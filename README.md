# SOC-Triage-Lab
Real-world SOC Level 1 Incident Triage Reports &amp; Attack Simulations
# Incident Triage Report: Spearphishing Attachment Execution (T1566.001)

## 1. Executive Summary
* **Incident ID:** INC-2026-08271
* **Severity:** High
* **Detection Source:** Windows Sysmon (Operational Logs)
* **Framework Mapping:** MITRE ATT&CK - [T1566.001 (Spearphishing Attachment)](https://attack.mitre.org/techniques/T1566/001)

---

## 2. Environment & System Context
* **Host System:** `WINDOWS-11`
* **User Context:** `WINDOWS-11\vboxuser`
* **Internal IP:** `192.168.100.52`

---

## 3. Incident Investigation & Evidence (Triage)

### Step 1: Adversary Execution Phase
An attack scenario was simulated using Atomic Red Team to download a malicious macro-enabled Excel document via PowerShell directly into the user's temporary directory (`%TEMP%`).

```powershell
$url = '[https://github.com/redcanaryco/atomic-red-team/raw/master/atomics/T1566.001/bin/PhishingAttachment.xlsm](https://github.com/redcanaryco/atomic-red-team/raw/master/atomics/T1566.001/bin/PhishingAttachment.xlsm)'
[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
Invoke-WebRequest -Uri $url -OutFile$env:TEMP\PhishingAttachment.xlsm
