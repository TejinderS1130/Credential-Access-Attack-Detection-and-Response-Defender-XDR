# SOC Incident Report: Credential Access Attack (Mimikatz)

---

## Incident Overview

**Severity:** High  
**Status:** Investigated & Contained  
**Analyst:** Tejinder Singh  
**Platform:** Microsoft Defender XDR  
**Environment:** Windows Endpoint (Azure VM)  

---

## Attack Description

This incident involves a **credential access attack** where a malicious payload associated with **Mimikatz** was downloaded onto a Windows endpoint.

Mimikatz is a well-known post-exploitation tool used to extract credentials from system memory (LSASS), making it a high-risk threat in enterprise environments.

### Attack Objective

- Execute a credential dumping tool  
- Extract sensitive authentication data from memory  
- Potentially use stolen credentials for lateral movement  

---

## Attack Flow

| Step | Activity |
|------|--------|
| 1 | User initiated a download from an external source (exploit-db) |
| 2 | Malicious file associated with Mimikatz delivered to endpoint |
| 3 | Microsoft Defender detected the file during download |
| 4 | Execution blocked before credential dumping |
| 5 | Incident automatically created in Defender XDR |

---

## Detection Summary

| Attribute | Details |
|----------|--------|
| Detection Source | Microsoft Defender Antivirus |
| Detection Type | Malware (Credential Dumping Tool) |
| Technique | T1003 – Credential Dumping |
| Status | Blocked before execution |

---

## SOC Investigation Methodology

### Step 1 — Alert Triage
- Alert triggered indicating malware prevention  
- Severity assessed based on credential dumping behavior  

---

### Step 2 — Incident Review
- Opened incident in Defender XDR  
- Reviewed alert details, affected endpoint, and timeline  

---

### Step 3 — Evidence Analysis
- Analyzed file hash, process activity, and alerts  
- Identified malicious file linked to Mimikatz  

---

### Step 4 — Threat Validation
- Validated file hash using VirusTotal  
- Confirmed malicious Mimikatz variant  

---

### Step 5 — Threat Hunting

DeviceFileEvents  
| where FileName contains "mimikatz"

**Findings:**
- Activity limited to a single endpoint  
- No additional affected devices identified  

---

### Step 6 — Investigation Graph Analysis
- Reviewed attack story in Defender XDR  
- Confirmed attack path: external source → file → endpoint  
- Verified no further propagation  

---

### Step 7 — Containment Verification
- Confirmed Defender blocked execution  
- No persistence mechanisms observed  
- No lateral movement detected  

---

## Response Actions

### Immediate Actions
- Malicious file blocked and quarantined  
- Endpoint secured  

---

### Remediation
- Verified system integrity  
- Ensured Defender signatures were up to date  

---

### IOC Blocking
- File hash added as indicator  
- Prevented future execution  

---

## Key Indicators of Compromise (IOCs)

| Indicator Type | Value |
|---------------|------|
| Tool | Mimikatz |
| Behavior | Credential dumping (LSASS access) |
| Source | exploit-db |
| Detection | Defender XDR alert |

---

## False Positives Consideration

### Potential False Positives
- Red team or penetration testing activity  
- Security research tools  
- Controlled lab environments  

---

### Validation Approach
- Verified file hash via VirusTotal  
- Correlated with process activity  
- Confirmed malicious behavior  

---

## Detection Engineering Insights

- Defender combines signature + behavioral detection  
- LSASS access is a strong credential dumping indicator  
- Threat intelligence improves detection confidence  
- Early detection prevents execution  

---

## Lessons Learned

- Credential dumping tools are high-risk pre-execution  
- Endpoint detection is critical for early prevention  
- Threat validation ensures accuracy  
- Proactive hunting confirms no hidden compromise  

---

## MITRE ATT&CK Mapping

| Technique ID | Name |
|-------------|------|
| T1003 | Credential Dumping |
| T1059 | Command Execution |
| T1204.001 | User Execution (Malicious Link) |

---

## Impact Assessment

| Metric | Value |
|-------|------|
| Devices Affected | 1 |
| Execution | Blocked |
| Persistence | None |
| Lateral Movement | None |
| Detection Time | Near real-time |
| Response Time | Immediate |

---

## SOC Analyst Summary

This incident demonstrates a **successful detection and containment of a credential access attack** using Microsoft Defender XDR.

The malicious payload was identified and blocked before execution, preventing credential theft and further compromise.

### Key Takeaways:

- Endpoint detection stops credential attacks early  
- Investigation + threat hunting ensures full visibility  
- IOC blocking strengthens security posture  
- Structured SOC workflows enable rapid response  

---
