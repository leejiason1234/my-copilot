# JZR-78392: DCM MQTT Upload DUPLICATE

<!-- Example of issue-template.md filled in for JZR-78392. Use this as a reference when creating a new context file from issue-template.md. -->

## Logs

| Log File | Type | Description |
|----------|------|-------------|
| `logs/Access Log(MQTT)-data-2025-11-12 16_39_37.csv` | Broker | MQTT broker access log — all PUBLISH/SUBSCRIBE/CONNECT/DISCONNECT events |
| `logs/JZR-78392_ServerLogs.csv` | Server | SMC-analysed server log — correlated request/response pairs with result codes |
| `logs/decode64_server_request_log.csv` | Raw | Base64-decoded server request messages |
| `logs/decode64_server_receive_log.csv` | Raw | Base64-decoded server receive (response) messages |

## Environment

| Field | Value |
|-------|-------|
| Platform / Hardware | A35 + A7 (DCM) |
| Component Under Test | DCM — MQTT stack, voice-call-app, HTTP upload |
| Protocol(s) | MQTT over mTLS (port 8883), HTTP/1.1 (result upload) |
| Test Type | Passive field test (intermittent cellular coverage) |

## Problem Statement

**Observed behavior**: DCM responded with result `DUPLICATE` for 3 server-initiated MQTT requests. The server retried each request after not receiving an `ACCEPTED` result within 30 seconds, causing DCM to classify repeat submissions as duplicates.

**Affected identifiers**:
- `6785cfe0-5246-4acb-88d8-de1634444536`
- `a2d5b30f-30ac-4d65-82ae-2fc749e5e73a`
- `9be08dfc-86d3-45b7-b1db-46d91ac532dc`

## Expected vs Actual Behavior

| | Description |
|--|-------------|
| **Expected** | DCM receives the MQTT request, publishes `ACCEPTED` back to the broker, forwards the request to voice-call-app, and uploads the HTTP result. Server receives `ACCEPTED` and does not retry. |
| **Actual** | DCM processed the first request and uploaded the HTTP result, but never published `ACCEPTED` to the broker. Server timed out after 30 s and retried twice. On the second and third delivery DCM returned `DUPLICATE`. |

## Analysis Result

**Root Cause Summary**: DCM's MQTT connection was in `DISCONNECTING` state when voice-call-app attempted to publish the `ACCEPTED` result. The outbound MQTT PUBLISH was held in a temporary queue with a 60 s TTL. The MQTT connection was not restored within 60 s, so the queued `ACCEPTED` packet was silently discarded. The server never received `ACCEPTED`, triggered its 30 s retry twice, and DCM responded `DUPLICATE` to both retries.

### Timeline: `6785cfe0-5246-4acb-88d8-de1634444536`

| Timestamp | Event | Source |
|-----------|-------|--------|
| 9/21/2025 8:59:09 AM | Server sends first MQTT request (correlation ID `6785cfe0…`) | Server log |
| 9/21/2025 8:59:13 AM | DCM receives request; voice-call-app uploads result via HTTP POST (no `ACCEPTED` published to broker) | Broker log / Server log |
| 9/21/2025 8:59:39 AM | Server retries — no `ACCEPTED` received within 30 s | Server log |
| 9/21/2025 9:00:09 AM | Server retries again (second retry) | Server log |
| 9/21/2025 9:00:15 AM | DCM publishes result `DUPLICATE` to broker | Broker log |

## Follow-Up Questions

### Q1: Why didn't DCM publish MQTT result ACCEPTED after the first request?

**Affected identifiers**: all three listed above  
**Source**: Broker log (`Access Log(MQTT)-data-…csv`); DCM source — MQTT publish queue, connection state machine  
**Initial finding**: DCM MQTT connection was in `DISCONNECTING` state at the time voice-call-app attempted to publish `ACCEPTED`. The publish was queued with a 60 s TTL. MQTT connection was not restored within 60 s, so the packet was discarded. `ACCEPTED` was therefore never passed to `MQTT_Abs_Publish_Async()`.  
**Status**: Partially answered — need to confirm from code whether the 60 s discard path is reached and whether there is a log statement emitted when a queued publish expires.

### Q2: Two HTTP uploads with same correlation ID but different body

**Affected identifiers**: `262238dd-aa89-4a10-8e21-9af66aff8747`  
**Source**: `logs/JZR-78392_ServerLogs.csv`  
**Initial finding**:  
**Status**: Not yet investigated.

### Q3: Why did DCM respond NOT_ACCEPT for some requests?

**Affected identifiers**: see `JZR-78392_ServerLogs.csv` for full list  
**Source**: `logs/JZR-78392_ServerLogs.csv`  
**Initial finding**:  
**Status**: Not yet investigated.

## Reproduction Steps

### Active Simulation Test (Preferred)
Force DCM into a *half-open TCP* state so it can receive inbound MQTT packets but all outbound packets are silently dropped.

### Pre-conditions
- DCM is in `MQTT Subscribed` state
- SSH/ADB access to DCM to run `iptables`

### Steps
1. Block outbound traffic to the MQTT broker: `iptables -A OUTPUT -p tcp --dport 8883 -j DROP`
2. Wait for DCM MQTT connection state to transition to `DISCONNECTING` (~120 s)
3. Trigger an MQTT push from the server to `<VIN>/C2V/OEMSW/remoteservices/oemgeneralinterface/eCall/centerRequest`
4. Observe the broker retransmit the same request twice more at 30 s intervals (server retry timeout)
5. When `WAIT TIMER expired` appears in the DCM log (UNSUBSCRIBE retry exhausted), remove the block: `iptables -D OUTPUT -p tcp --dport 8883 -j DROP`

### Expected Result
- DCM receives the MQTT request and forwards it to voice-call-app
- voice-call-app uploads the result via HTTP POST
- No `ACCEPTED` appears in the server log
- DCM publishes `DUPLICATE` and `DISCONNECT` simultaneously after the server retries

### Observed Result
Matches expected result above — this is the reproduction of the reported field behaviour.

---

### Passive Field Test (Alternative)
Place DCM in an area with intermittent cellular coverage (e.g. garage entry). Run a continuous MQTT push loop with 180 s intervals and collect logs every hour.
