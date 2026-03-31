# [TICKET-ID]: [Short Issue Title]

<!-- TODO: Replace TICKET-ID (e.g. JZR-78392) and write a 5-10 word title describing the symptom -->

## Logs

<!-- TODO: Add one row per log file. Remove rows that don't apply. -->

| Log File | Type | Description |
|----------|------|-------------|
| `logs/[filename].csv` | Device / Broker / Server / Raw | <!-- TODO: brief description --> |
| `logs/[filename].csv` | Device / Broker / Server / Raw | <!-- TODO: brief description --> |

## Environment

<!-- TODO: Fill in all fields that are known. Remove rows that don't apply. -->

| Field | Value |
|-------|-------|
| SW Version | <!-- TODO: e.g. v2.3.1 --> |
| Platform / Hardware | <!-- TODO: e.g. A35, Raspberry Pi 4 --> |
| Component Under Test | <!-- TODO: e.g. DCM, voice-call-app, broker --> |
| Protocol(s) | <!-- TODO: e.g. MQTT over mTLS, HTTP/1.1 --> |
| Test Type | <!-- TODO: Field / Lab / Simulation --> |

## Problem Statement

<!-- TODO: Describe the observed symptom in 2-4 sentences. What was expected vs. what happened? -->

**Observed behavior**: 

**Affected identifiers** (correlation IDs, session IDs, request IDs, etc.):
<!-- TODO: List each affected ID on its own line -->
- `<identifier-1>`
- `<identifier-2>`

## Expected vs Actual Behavior

| | Description |
|--|-------------|
| **Expected** | <!-- TODO: What the system should do --> |
| **Actual** | <!-- TODO: What the system actually did --> |

## Analysis Result

<!-- TODO: Fill in after investigation. Leave as-is if investigation is not yet started. -->

<!-- TODO: Summarize the root cause in 1-2 sentences once known -->
**Root Cause Summary**: Not yet determined.

### Timeline: `<identifier-1>`

<!-- TODO: Add one row per observable event from any log source. Sort chronologically. -->

| Timestamp | Event | Source |
|-----------|-------|--------|
| <!-- TODO --> | <!-- TODO --> | <!-- TODO: log file or component name --> |

<!-- TODO: Duplicate this table for each additional identifier if their timelines differ -->

## Follow-Up Questions

<!-- TODO: Add one section per open question. Remove examples that don't apply. -->

### Q1: [Question title]
<!-- TODO: Write the question as a single sentence -->

**Affected identifiers**: <!-- TODO: list IDs or "all" -->  
**Source**: <!-- TODO: which log file or code area to look in -->  
**Initial finding**: <!-- TODO: leave blank until partially answered -->  
**Status**: Not yet investigated.

### Q2: [Question title]

**Affected identifiers**:  
**Source**:  
**Initial finding**:  
**Status**: Not yet investigated.

### Q3: [Question title]

**Affected identifiers**:  
**Source**:  
**Initial finding**:  
**Status**: Not yet investigated.

## Reproduction Steps

<!-- TODO: Fill in once the root cause is understood well enough to reproduce -->

### Pre-conditions
<!-- TODO: List the system state required before triggering the bug -->
- 

### Steps
<!-- TODO: Numbered, actionable steps -->
1. 

### Expected Result
<!-- TODO: What the system should do if working correctly -->

### Observed Result
<!-- TODO: What the system actually does -->
