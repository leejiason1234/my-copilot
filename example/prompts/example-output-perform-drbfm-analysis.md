# DRBFM Analysis: JZR-79238 Program Version Update

## Behavior Before Change

The USER_AGENT PROGRAM_VERSION field defaulted to '0' (position 6 in USER_AGENT string), resulting in USER_AGENT string "ZZCINA0000" for all model years.

## Behaviour After Change

The USER_AGENT PROGRAM_VERSION field defaults to '1' (position 6 in USER_AGENT string), resulting in USER_AGENT string "ZZCINA1000" to differentiate MY26 vehicles from MY24 (which remains '0').

## How is it changed

Modified XML configuration file config_conncore_map.xml:44 to change PROGRAM_VERSION SUB_CID default_value from "0" to "1". Updated all unit test cases in dcm_information_configurator_test.cpp to reflect new default USER_AGENT value from "ZZCINA0000" to "ZZCINA1000".

---

## Concern 1

### Condition Occurence
Configuration file cache or persistence storage retains old default value "0" after software update from MY24 to MY26 version.

### System Failure caused by concern
Backend server misidentifies MY26 vehicle as MY24 variant due to incorrect PROGRAM_VERSION field in USER_AGENT string.

### Cause of concern
- Persistence layer does not invalidate cached configuration on software version change
- XML configuration defaultOnVersion="false" prevents default value reapplication during version upgrade
- Flash write failure during configuration update

### Impact to end user
Vehicle receives incorrect over-the-air updates, feature enablement, or service configurations intended for MY24 instead of MY26.

### Preventive Measure

#### System Design Measure
NA

#### Software Design Measure
Modify configuration manager to implement version-aware default value enforcement: when software version changes are detected, force reapplication of defaults for version-critical fields regardless of defaultOnVersion flag. Implement configuration migration script that validates and updates PROGRAM_VERSION field during FOTA upgrade from MY24 to MY26 builds.

#### Evaluation Measure
Perform verification & validation

**TC:** Verify PROGRAM_VERSION field updates correctly when upgrading from MY24 software to MY26 software.

**Precondition(s)**
- Flash MY24 software build with PROGRAM_VERSION default "0"
- Verify USER_AGENT persisted as "ZZCINA0000"

**Test Steps:**
1. Perform FOTA update to MY26 software build
2. Read USER_AGENT from configuration after update
3. Verify PROGRAM_VERSION field value

**Expected Result:**
- USER_AGENT reads "ZZCINA1000" with PROGRAM_VERSION='1'
- Backend server correctly identifies vehicle as MY26 variant

#### Manufacturing Measure
Factory test procedure to verify USER_AGENT string matches expected MY26 default "ZZCINA1000" before vehicle shipment.

---

## Concern 2

### Condition Occurence
Unit test suite contains hardcoded USER_AGENT values that become outdated when default changes in future model years.

### System Failure caused by concern
Test maintenance burden increases; regression risk when tests pass with incorrect expectations.

### Cause of concern
Test data hardcoded as character arrays instead of deriving from same configuration source as production code. No compile-time validation that test fixtures match actual configuration defaults.

### Impact to end user
Defects escape testing when unit tests validate against stale expected values, potentially causing field failures.

### Preventive Measure

#### System Design Measure
NA

#### Software Design Measure
Refactor unit tests to use configuration file parser to extract default values at test initialization, eliminating hardcoded test data. Implement build-time configuration validation that fails compilation if test fixture USER_AGENT values don't match XML configuration defaults.

#### Evaluation Measure
Code review to verify test data derivation strategy.

Static analysis check: Grep unit test files for hardcoded USER_AGENT patterns and flag as code review findings.

#### Manufacturing Measure
NA

---

## Concern 3

### Condition Occurence
Backend systems or diagnostic tools parse PROGRAM_VERSION field expecting single-digit ASCII '0'-'9' range but receive out-of-range value in future updates.

### System Failure caused by concern
Server-side parsing failure or integer overflow when converting ASCII character to numeric version identifier.

### Cause of concern
XML configuration defines PROGRAM_VERSION as single-character ASCII with range 0-255, but semantic meaning limited to version number. No enforcement of numeric-only values. Future MY updates could exhaust single-digit range.

### Impact to end user
Vehicle-to-server communication failures when backend rejects malformed USER_AGENT strings.

### Preventive Measure

#### System Design Measure
Define PROGRAM_VERSION field evolution strategy: document maximum supported model years (0-9 allows 10 variants). Specify migration plan to multi-character version field when single-digit exhausted.

#### Software Design Measure
Add XML schema validation to restrict PROGRAM_VERSION default_value to ASCII digits '0'-'9' only. Implement runtime validation in configuration manager to reject non-numeric PROGRAM_VERSION values. Update documentation specifying PROGRAM_VERSION encoding scheme and range limits.

#### Evaluation Measure
Perform verification & validation

**TC:** Verify configuration manager rejects invalid PROGRAM_VERSION values.

**Precondition(s)**
- Modify USER_AGENT PROGRAM_VERSION to non-numeric character (e.g., 'X')

**Test Steps:**
1. Initialize DCM configuration manager
2. Attempt to read USER_AGENT
3. Check configuration status and error logs

**Expected Result:**
- Configuration manager returns validation error
- System reverts to last known-good configuration or safe default
- Diagnostic trouble code logged

#### Manufacturing Measure
NA