# Screenshots & Telemetry Evidence Index

This directory contains the visual evidence and technical screenshots captured during the triage of **Incident INC-2026-08271** (Spearphishing Attachment Execution - T1566.001).

---

## Evidence Breakdown

| File Name | Category | Description |
| :--- | :--- | :--- |
| `01_powershell_execution.png` | **Execution** | PowerShell script execution downloading `PhishingAttachment.xlsm`. |
| `02_go-to-sysmon.png` | **Navigation** | Accessing Sysmon Operational logs via Windows Event Viewer. |
| `03_sysmon_event03.png` | **Telemetry** | Sysmon Event ID 3 (Network Connection) showing egress traffic to GitHub IP (`140.82.121.3:443`). |
| `04_sysmon_event11.png` | **Telemetry** | Sysmon Event ID 11 (File Create) showing binary creation in `%TEMP%`. |
| `05_temp_dropped_file.png` | **Artifact** | Verification of the dropped file location inside `C:\Users\vboxuser\AppData\Local\Temp\`. |

---
*Note: All artifacts were captured in a controlled Virtual Machine environment for analysis purposes.*
