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



## Step 2: Telemetry & Log Analysis (Sysmon)
### A. Outbound Network Connection (Sysmon Event ID 3)
Sysmon captured an outbound network connection initiated by PowerShell to fetch the malicious document over HTTPS (Port 443).

    Timestamp: 2026-08-27 06:51:26.106 UTC

    Process: powershell.exe (PID: 10024) 

    Path: C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe

     Source: 192.168.100.52:29372

     Destination: 140.82.121.3:443 (lb-140-82-121-3-fra.github.com)

###   B. File Creation Detection (Sysmon Event ID 11)
Sysmon captured the file creation event as powershell.exe dropped the macro-enabled binary into the local temp folder.

Timestamp: 2026-08-27 06:51:47.359 UTC

Process: powershell.exe (PID: 10024)

Target Filename: C:\Users\vboxuser\AppData\Local\Temp\PhishingAttachment.xlsm

Creation Time: 2026-08-22 17:02:17.788

##        4. Verdict & Root Cause Analysis
           Verdict: True Positive (Drive-by Download / Phishing Artifact Dropper Simulation).

            Root Cause: Script/User execution via powershell.exe fetching a macro-enabled Excel document (.xlsm) from an external GitHub repository into an unmonitored temporary user location (%TEMP%).

##           5. Containment & Remediation Plan
Artifact Removal: Purged the malicious file PhishingAttachment.xlsm from C:\Users\vboxuser\AppData\Local\Temp\.

Process Suppression: Terminated active instances of powershell.exe (PID 10024).

Detection Engineering: Deployed a SIEM correlation rule flagging powershell.exe initiating outbound network connections that drop Office files (.xlsm, .docm) into %TEMP% directories.

Endpoint Hardening: Recommended enforcing PowerShell Constrained Language Mode and Microsoft Defender Attack Surface Reduction (ASR) rules.











