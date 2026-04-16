# Detection Rules (KQL)

This file contains detection queries designed to identify credential access activity related to tools such as **Mimikatz**.

These detections simulate how SOC analysts create rules to identify suspicious behavior, trigger alerts, and support incident response workflows.

---

## Detection Objectives

- Detect credential dumping tools (e.g., Mimikatz)  
- Identify suspicious process execution patterns  
- Monitor LSASS access attempts  
- Correlate behavioral indicators with known attack techniques  

---

## Detection Rule 1 — Mimikatz File Detection

DeviceFileEvents  
| where FileName contains "mimikatz"

**Detection Logic:**  
Identifies file events associated with Mimikatz across endpoints.

**Trigger Condition:**  
File name matches known credential dumping tools.

**MITRE Mapping:**  
T1003 — Credential Dumping

---

## Detection Rule 2 — Suspicious Process Execution

DeviceProcessEvents  
| where ProcessCommandLine contains "mimikatz"

**Detection Logic:**  
Detects execution of processes related to Mimikatz.

**Trigger Condition:**  
Command-line arguments indicate credential dumping activity.

**MITRE Mapping:**  
T1059 — Command Execution

---

## Detection Rule 3 — LSASS Access Monitoring

DeviceImageLoadEvents  
| where InitiatingProcessFileName =~ "mimikatz.exe"

**Detection Logic:**  
Monitors processes interacting with LSASS memory.

**Trigger Condition:**  
Suspicious process attempting credential access.

**MITRE Mapping:**  
T1003 — Credential Dumping

---

## Detection Rule 4 — Credential Dumping Commands

DeviceProcessEvents  
| where ProcessCommandLine contains "sekurlsa"

**Detection Logic:**  
Detects usage of Mimikatz modules related to credential extraction.

**Trigger Condition:**  
Presence of credential dumping commands.

**MITRE Mapping:**  
T1003 — Credential Dumping

---

## Detection Rule 5 — Broad Credential Dumping Behavior

DeviceProcessEvents  
| where ProcessCommandLine has_any ("mimikatz", "sekurlsa", "logonpasswords")

**Detection Logic:**  
Captures variations of credential dumping activity.

**Trigger Condition:**  
Multiple indicators of suspicious command execution.

**MITRE Mapping:**  
T1003 — Credential Dumping

---

## False Positives Consideration

### Potential False Positives
- Security testing tools used in lab environments  
- Red team or penetration testing activity  
- Administrative scripts with similar patterns  

---

### Tuning Strategies
- Filter known safe processes or trusted hosts  
- Baseline normal process behavior  
- Combine with additional indicators (e.g., user, time, frequency)  

---

## Detection Engineering Insights

- Behavioral detection improves accuracy beyond signatures  
- Command-line analysis is critical for identifying attacks  
- Combining multiple indicators increases detection confidence  
- Detection rules should be continuously tuned to reduce noise  

---

## SOC Value

These detection rules demonstrate:

- Ability to create SOC-ready detection logic  
- Understanding of attacker behavior (TTPs)  
- Mapping detections to MITRE ATT&CK  
- Practical implementation of detection engineering principles  

---
