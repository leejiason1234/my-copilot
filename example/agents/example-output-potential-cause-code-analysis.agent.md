User: analyze JZR-78392. What could possible reason where voice call from INDIA variant uploaded msd with same corlleration id on 2nd MSD trigger.

GitHub Copilot: Searched for files matching `**/.github/agents/context/jzr-78392*`, 1 match

Read [](file:///home/uidn8804/Workspace/Suzuki/JZR-78392-DCM-MQTT-UPLOAD-DUPLICATE/.github/agents/context/jzr-78392-analysis.md)

Created 3 todos

Searched for text `262238dd-aa89-4a10-8e21-9af66aff8747` (`**/logs/**`), 17 results

Searched for text `INDIA|india|variant` (`**/logs/**`), no results

Read [](file:///home/uidn8804/Workspace/Suzuki/JZR-78392-DCM-MQTT-UPLOAD-DUPLICATE/logs/decode64_server_receive_log.csv#25-25), lines 25 to 35

Read [](file:///home/uidn8804/Workspace/Suzuki/JZR-78392-DCM-MQTT-UPLOAD-DUPLICATE/logs/JZR-78392_ServerLogs.csv#28-28), lines 28 to 80

Read [](file:///home/uidn8804/Workspace/Suzuki/JZR-78392-DCM-MQTT-UPLOAD-DUPLICATE/logs/JZR-78392_ServerLogs.csv#74-74), lines 74 to 160

Read [](file:///home/uidn8804/Workspace/Suzuki/JZR-78392-DCM-MQTT-UPLOAD-DUPLICATE/logs/decode64_server_receive_log.csv#125-125), lines 125 to 180

Now let me trace the voice call code path — specifically how the correlationId is stored and how MSD uploads are re-triggered.  

Read [](file:///home/uidn8804/Workspace/Suzuki/JZR-78392-DCM-MQTT-UPLOAD-DUPLICATE/voice-call-app/code)

Searched for text `correlationId` (`**/voice-call-app/**`), 20 results

Read [](file:///home/uidn8804/Workspace/Suzuki/JZR-78392-DCM-MQTT-UPLOAD-DUPLICATE/voice-call-app/code/Adapters/CommunicationManager/CommunicationMgr.cpp#120-120), lines 120 to 200

Searched for text `m_currentPostCorrelationId` (`**/voice-call-app/**`), 20 results

Read [](file:///home/uidn8804/Workspace/Suzuki/JZR-78392-DCM-MQTT-UPLOAD-DUPLICATE/voice-call-app/code/Adapters/CommunicationManager/CommunicationMgr.cpp#855-855), lines 855 to 960

Read [](file:///home/uidn8804/Workspace/Suzuki/JZR-78392-DCM-MQTT-UPLOAD-DUPLICATE/voice-call-app/code/Adapters/CommunicationManager/CommunicationMgr.cpp#1040-1040), lines 1040 to 1160

Searched for text `sendEmergencyDataToServer` (`**/voice-call-app/**`), 20 results

Searched for text `VC_MSG_BACKEND_CALL_TRIGGERED` (`**/voice-call-app/**`), 7 results

Read [](file:///home/uidn8804/Workspace/Suzuki/JZR-78392-DCM-MQTT-UPLOAD-DUPLICATE/voice-call-app/code/Core/StateMachines/Voice_call_state_machine_OEM.cpp#800-800), lines 800 to 870

Read [](file:///home/uidn8804/Workspace/Suzuki/JZR-78392-DCM-MQTT-UPLOAD-DUPLICATE/voice-call-app/code/Core/StateMachines/Voice_call_state_machine_OEM.cpp#1745-1745), lines 1745 to 1820

Searched for text `updateAndResendMsd` (`**/voice-call-app/**`), 9 results

Read [](file:///home/uidn8804/Workspace/Suzuki/JZR-78392-DCM-MQTT-UPLOAD-DUPLICATE/voice-call-app/code/Core/StateMachines)

Read [](file:///home/uidn8804/Workspace/Suzuki/JZR-78392-DCM-MQTT-UPLOAD-DUPLICATE/voice-call-app/code/Core/StateMachines/Suzuki)

Searched for text `INDIA|india|INDIAStateMachine|IndiaSM` (`**/voice-call-app/code/Core/StateMachines/Suzuki/**`), 20 results

Searched for regex `INDIAStateMachine\|IndiaStateMachine\|India_state\|INDIA_SM` (`**/voice-call-app/**`), no results

Searched for text `VC_EV_START_BACKEND_CALL` (`**/voice-call-app/code/Core/StateMachines/Suzuki/**`), 2 results

Searched for regex `DATA_TRANSMISSION|sendData|MSD_PERIODICAL|T_WAIT|WFC|WaitForCallback` (`**/voice-call-app/code/Core/StateMachines/Suzuki/**`), 20 results

Read [](file:///home/uidn8804/Workspace/Suzuki/JZR-78392-DCM-MQTT-UPLOAD-DUPLICATE/voice-call-app/code/Core/StateMachines/Suzuki/Voice_call_state_machine_OEM.cpp#270-270), lines 270 to 360

Read [](file:///home/uidn8804/Workspace/Suzuki/JZR-78392-DCM-MQTT-UPLOAD-DUPLICATE/voice-call-app/code/Core/StateMachines/Suzuki/Voice_call_state_machine_OEM.cpp#100-100), lines 100 to 180

Read [](file:///home/uidn8804/Workspace/Suzuki/JZR-78392-DCM-MQTT-UPLOAD-DUPLICATE/voice-call-app/code/Core/StateMachines/Suzuki/Voice_call_state_machine_OEM.cpp#1420-1420), lines 1420 to 1820

Searched for text `handleDataResponse` (`**/voice-call-app/code/Core/StateMachines/Suzuki/**`), 9 results

Searched for text `goToWFC\|goToDataTransmission\|WFC\|WaitForCallback\|VC_EV_VOICE_READY_TIMER` (`**/voice-call-app/code/Core/StateMachines/Suzuki/**`), no results

Read [](file:///home/uidn8804/Workspace/Suzuki/JZR-78392-DCM-MQTT-UPLOAD-DUPLICATE/voice-call-app/code/Core/StateMachines/Suzuki/Voice_call_state_machine_OEM.cpp#3529-3529), lines 3529 to 3620

Read [](file:///home/uidn8804/Workspace/Suzuki/JZR-78392-DCM-MQTT-UPLOAD-DUPLICATE/voice-call-app/code/Adapters/CommunicationManager/CommunicationMgr.cpp#1000-1000), lines 1000 to 1080

Searched for regex `callState\.second|callState\.first` (`**/voice-call-app/code/Core/StateMachines/Suzuki/**`), 15 results

Searched for regex `callState\s*=.*pair|pair.*callState` (`**/voice-call-app/code/Core/**`), 20 results

Searched for text `smChangeState_OEM\|goToWFC\|smChangeState` (`**/voice-call-app/code/Core/StateMachines/Suzuki/**`), no results

Searched for text `smChangeState_OEM` (`**/voice-call-app/**`), 9 results

Searched for text `goToWFC` (`**/voice-call-app/code/Core/StateMachines/Suzuki/**`), 10 results

Read [](file:///home/uidn8804/Workspace/Suzuki/JZR-78392-DCM-MQTT-UPLOAD-DUPLICATE/voice-call-app/code/Core/StateMachines/Suzuki/Voice_call_state_machine_OEM.cpp#2451-2451), lines 2451 to 2520

Read [](file:///home/uidn8804/Workspace/Suzuki/JZR-78392-DCM-MQTT-UPLOAD-DUPLICATE/voice-call-app/code/Core/StateMachines/Suzuki/Voice_call_state_machine_OEM.cpp#2520-2520), lines 2520 to 2610

Searched for text `triggerEcall` (`**/voice-call-app/code/Core/StateMachines/Suzuki/**`), 6 results

Read [](file:///home/uidn8804/Workspace/Suzuki/JZR-78392-DCM-MQTT-UPLOAD-DUPLICATE/voice-call-app/code/Core/StateMachines/Suzuki/Voice_call_state_machine_OEM.cpp#412-412), lines 412 to 520

Read [](file:///home/uidn8804/Workspace/Suzuki/JZR-78392-DCM-MQTT-UPLOAD-DUPLICATE/logs/Access%20Log%28MQTT%29-data-2025-11-12%2016_39_37.csv#1-1), lines 1 to 100

Searched for text `eCall\|centerRequest\|ecall` (`**/logs/**`), no results

Read [](file:///home/uidn8804/Workspace/Suzuki/JZR-78392-DCM-MQTT-UPLOAD-DUPLICATE/logs/Access%20Log%28MQTT%29-data-2025-11-12%2016_39_37.csv#1-1), lines 1 to 200

Read [](file:///home/uidn8804/Workspace/Suzuki/JZR-78392-DCM-MQTT-UPLOAD-DUPLICATE/voice-call-app/code/Adapters/CommunicationManager/CommunicationMgr.cpp#807-807), lines 807 to 870

Completed: *Read server logs for correlation ID 262238dd* (1/3)

---

### Timeline

| Timestamp | Event | Source | Identifier |
|-----------|-------|--------|------------|
| `2025-09-21 08:06:57` | Server sends MQTT command `02` (callback / start voice call) to DCM | decode64_server_request_log.csv | corrId: `262238dd-aa89-4a10-8e21-9af66aff8747` |
| `2025-09-21 08:06:58` | DCM sends MQTT ACCEPT back to server | JZR-78392_ServerLogs.csv (#5) | corrId: `262238dd` |
| `2025-09-21 08:07:01` | DCM uploads **first MSD** via HTTP (triggerId:1, notificationCounter:25, dateTime:08:06:58) | decode64_server_receive_log.csv | corrId: `262238dd` |
| `2025-09-21 08:07:01` | Server responds HTTP 200 to MSD upload | decode64_server_receive_log.csv | corrId: `262238dd` |
| `2025-09-21 08:07:01` | `onPostHttpResponseReceived`: India-specific early `break` fires — **`m_currentPostCorrelationId` is NOT cleared** | CommunicationMgr.cpp:855 | — |
| `2025-09-21 08:10:36` | IG ON trip notification — call has ended, DCM back in WFC | JZR-78392_ServerLogs.csv (#15) | — |
| `2025-09-21 08:55:07` | IG ON notification | JZR-78392_ServerLogs.csv (#16) | — |
| `2025-09-21 08:57:41` | DCM uploads **second MSD** via HTTP (triggerId:1, notificationCounter:36, dateTime:08:57:39, **different body**), reusing the stale `262238dd` correlation ID | decode64_server_receive_log.csv | corrId: `262238dd` |
| `2025-09-21 08:59:07` | Server sends **new** MQTT eCall requests — a completely different eCall session begins | MQTT broker log | corrId: `6785cfe0`, `a2d5b30f`, `9be08dfc` |

---

### Root Cause

In `CommunicationMgr::onPostHttpResponseReceived` (CommunicationMgr.cpp), the India-variant (`REGION_CODE_IN`) success path contains an early `break` (at CommunicationMgr.cpp) that exits the `do-while` block **before** the `memset` at CommunicationMgr.cpp that is supposed to clear `m_currentPostCorrelationId`. As a result, after the first MSD HTTP upload succeeds, `m_currentPostCorrelationId` still holds the correlation ID from the original MQTT trigger (`262238dd`). When new MSD upload is triggered ~50 minutes later by an independent eCall trigger, `sendEmergencyDataToServer` checks `strlen(m_currentPostCorrelationId.uuid_str) == 0` at CommunicationMgr.cpp — finds it non-empty — and **reuses the stale correlationId** instead of generating a new UUID.

---

### Evidence

- **[CommunicationMgr.cpp line 851–857](voice-call-app/code/Adapters/CommunicationManager/CommunicationMgr.cpp#L851)**: India success path:
  ```cpp
  if(REGION_CONFIG_CODE::REGION_CODE_IN == m_pCommMgr->m_fdbParam.tcp_region_code)
  {
      // publishes VC_MSG_DATA_RESPONSE_RECEIVED
      break;   // ← exits do-while; m_currentPostCorrelationId is never cleared
  }
  ```
- **[CommunicationMgr.cpp line 929](voice-call-app/code/Adapters/CommunicationManager/CommunicationMgr.cpp#L929)**: The reset is unreachable for India:
  ```cpp
  memset(&m_pCommMgr->m_currentPostCorrelationId.uuid_str[0], 0, sizeof(...));
  ```
- **[CommunicationMgr.cpp line 139](voice-call-app/code/Adapters/CommunicationManager/CommunicationMgr.cpp#L139)**: Stale value re-used on next trigger:
  ```cpp
  if(strlen(m_currentPostCorrelationId.uuid_str) == 0)  // ← false; no new UUID generated
  ```
- `decode64_server_receive_log.csv:08:07:01` — First MSD uploaded with `corrId=262238dd`, HTTP 200 returned
- `decode64_server_receive_log.csv:08:57:41` — Second MSD uploaded **50 min 40 s later** with the **same** `corrId=262238dd`; different `Data`, `dateTime`, `notificationCounter` (36 vs 25) — confirmed independent trigger
- MQTT broker log: **no** eCall C2V MQTT message exists between `08:07` and `08:59:07` — rules out server re-request as the cause

---

### Fix Recommendation

`[CODE]` In `CommunicationMgr::onPostHttpResponseReceived` ([CommunicationMgr.cpp line 851](voice-call-app/code/Adapters/CommunicationManager/CommunicationMgr.cpp#L851)), clear `m_currentPostCorrelationId` **before** the India-path `break`:

```cpp
if(REGION_CONFIG_CODE::REGION_CODE_IN == m_pCommMgr->m_fdbParam.tcp_region_code)
{
    commMgrLogger() << INFO << __func__ << ": HTTP POST request completed successfully";
    // Clear the correlation ID so the next independent trigger generates a fresh UUID
    memset(&m_pCommMgr->m_currentPostCorrelationId.uuid_str[0], 0,
           sizeof(m_pCommMgr->m_currentPostCorrelationId.uuid_str));
    VoiceCallMessagesPayloads msgPayload = VoiceCallMessagesPayloads::VC_PAYLOAD_HTTP_RESPONSE_RECEIVED;
    alx::ControllerIf::getController().publish(VC_MSG_DATA_RESPONSE_RECEIVED,
        reinterpret_cast<uint8_t*>(&msgPayload), sizeof(uint8_t));
    break;
}
```

---

### Reproduction Steps

**Pre-conditions**: India variant DCM in `VC_WAITING_FOR_CALLBACK` state (prior eCall ended, T9 timer running)

**Steps**:
1. Server sends MQTT command `02` (callback) to DCM — DCM accepts and uploads MSD: correlation ID X is stored in `m_currentPostCorrelationId`
2. Server returns HTTP 200 for the MSD upload — `onPostHttpResponseReceived` India path fires, `break`s early, **does not clear** `m_currentPostCorrelationId`
3. Voice call ends; DCM returns to `VC_WAITING_FOR_CALLBACK` with `m_currentPostCorrelationId` still = X
4. Any new eCall trigger occurs (manual button, automatic, another server callback, etc.)
5. `sendEmergencyDataToServer` is called; `strlen(m_currentPostCorrelationId) != 0` → stale X is reused

**Expected result**: Each independent MSD upload carries a freshly generated UUID as its correlation ID  
**Observed result**: Second MSD upload reuses correlation ID `262238dd-aa89-4a10-8e21-9af66aff8747` from the first eCall session, confirmed at `08:57:41` vs `08:07:01` (50 min 40 s gap, different MSD body)