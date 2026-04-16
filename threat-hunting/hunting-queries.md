# Threat Hunting Queries (KQL)

This file contains proactive threat hunting queries used to identify suspicious activity related to credential access attacks.

These queries were used during the investigation to proactively hunt for credential dumping activity and validate that no additional endpoints were compromised.

---

## Objective

To identify indicators of credential dumping activity (e.g., Mimikatz) and detect potential threats that may not have triggered alerts.

---

## Hunt for Mimikatz File Activity

DeviceFileEvents  
| where FileName contains "mimikatz"

**Purpose:**  
Identify file events related to Mimikatz across all endpoints.

**SOC Insight:**  
Helps detect known credential dumping tools, even if alerts were missed.

---

## Hunt for Suspicious Process Execution

DeviceProcessEvents  
| where ProcessCommandLine contains "mimikatz"

**Purpose:**  
Detect execution of processes associated with Mimikatz.

**SOC Insight:**  
Captures command-line execution patterns commonly used in credential dumping attacks.

---

## Hunt for LSASS Access Attempts

DeviceImageLoadEvents  
| where InitiatingProcessFileName =~ "mimikatz.exe"

**Purpose:**  
Identify attempts to interact with LSASS (credential storage process).

**SOC Insight:**  
LSASS access is a strong indicator of credential dumping behavior.

---

## Hunt for Suspicious Command Patterns

DeviceProcessEvents  
| where ProcessCommandLine contains "sekurlsa"

**Purpose:**  
Detect usage of Mimikatz modules related to credential extraction.

**SOC Insight:**  
Even if file names are changed, command patterns can reveal malicious intent.

---

## Hunt Across All Endpoints

DeviceProcessEvents  
| where ProcessCommandLine has_any ("mimikatz", "sekurlsa", "logonpasswords")

**Purpose:**  
Provide broader detection of credential dumping behavior.

**SOC Insight:**  
Captures variations of Mimikatz usage and potential obfuscation techniques.

---

## Analyst Notes

- All queries returned activity limited to a single endpoint  
- No evidence of lateral movement was observed  
- No additional compromised devices were identified  
- The environment was validated as secure after investigation  

---

## SOC Value

This demonstrates proactive threat hunting beyond alert-based detection:

- Identifies hidden threats  
- Confirms scope of compromise  
- Validates containment effectiveness  
- Enhances overall detection capability  

---
