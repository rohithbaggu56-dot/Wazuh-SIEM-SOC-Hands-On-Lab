# 🔐 Wazuh SIEM – Hands-On SOC Lab (Windows & Linux)

![Wazuh](https://img.shields.io/badge/Wazuh-005DAC?style=for-the-badge&logo=wazuh&logoColor=white)
![Sysmon](https://img.shields.io/badge/Sysmon-E65100?style=for-the-badge)
![VirusTotal](https://img.shields.io/badge/VirusTotal-394EFF?style=for-the-badge)
![Windows](https://img.shields.io/badge/Windows-0078D6?style=for-the-badge&logo=windows&logoColor=white)
![Kali Linux](https://img.shields.io/badge/Kali_Linux-557C94?style=for-the-badge&logo=kalilinux&logoColor=white)
![MITRE ATT&CK](https://img.shields.io/badge/MITRE_ATT%26CK-FF0000?style=for-the-badge)

A hands-on Wazuh deployment across Windows 10 and Linux endpoints with Sysmon integration, VirusTotal API active response pipeline, MITRE ATT&CK mapping, and false positive tuning. Every detection shown here was triggered by real activity in the lab.

---

## 🖥️ Lab Environment

| Machine | Role |
|---|---|
| Ubuntu Server | Wazuh manager, log collection, active response host |
| Windows 10 | Wazuh agent, Sysmon monitoring, endpoint detection |
| Kali Linux | Wazuh agent, malware simulation, attack source |

---

## 📂 What Was Built and Detected

---

### 🔴 VirusTotal API + Active Response Pipeline

Configured Wazuh to automatically detect and delete malware using the VirusTotal API. When a file is dropped, Sysmon catches it, Wazuh sends the hash to VirusTotal, and if the verdict is malicious, remove-threat.sh deletes the file automatically.

**ossec.conf — VirusTotal integration block configured on the Wazuh manager:**

<img width="1911" height="1080" alt="ossec_file" src="https://github.com/user-attachments/assets/b8dccf4f-1bbd-41cc-addc-c015e51c92af" />


**ossec.conf on Windows agent — Sysmon log source and active response command configured:**

<img width="1920" height="1080" alt="Screenshot 2026-03-24 102236" src="https://github.com/user-attachments/assets/517571a6-c60d-4446-b0b9-7e326b6e6de8" />


**ossec.conf — remove-threat command and active-response block wired to rule 87105:**

<img width="1911" height="1080" alt="Screenshot 2026-03-28 104822" src="https://github.com/user-attachments/assets/3482d91a-e5cb-4817-b6d3-6d43fa2d8319" />


**Wazuh alert showing VirusTotal returned 66/68 engine detections for the EICAR test file:**

<img width="1911" height="1080" alt="Screenshot 2026-03-28 100558" src="https://github.com/user-attachments/assets/98d341a4-2293-411f-b16b-6c8df4ae42e7" />


**VirusTotal website confirming the EICAR file verdict — 66 out of 68 engines flagged it:**

<img width="1911" height="1080" alt="Screenshot 2026-03-28 100630" src="https://github.com/user-attachments/assets/ace91d55-9051-4152-9b83-7ab7b8b49fb2" />


**Wazuh event log showing the full pipeline: file added, VirusTotal alert (rule 87105), active response triggered (rule 657), file deleted (rule 553):**

<img width="1911" height="1080" alt="Screenshot 2026-03-28 110356" src="https://github.com/user-attachments/assets/cf91a77c-cf45-4eeb-993b-483f299f9975" />


**MITRE ATT&CK events view confirming file deletion sequence — T1070.004 Defense Evasion, rule 553 firing:**

 <img width="1911" height="1080" alt="Screenshot 2026-03-24 130258" src="https://github.com/user-attachments/assets/c9c435e4-af81-4bf5-90d9-6e07db020059" />


- 📌 MITRE: `T1105` Ingress Tool Transfer · `T1070.004` File Deletion · `T1059` Command & Scripting Interpreter
  <img width="1911" height="1080" alt="Screenshot 2026-03-24 130258" src="https://github.com/user-attachments/assets/3b6424f0-16e4-4d06-89fc-8c97f02c8b37" />

---

### 🔴 Sysmon Integration & False Positive Tuning

Deployed Sysmon on Windows 10 with a custom XML config. Wazuh was generating T1105 false positives from Microsoft Edge downloading filter list updates. Identified the pattern and wrote a local_rules.xml suppression rule.

**Sysmon Event ID 11 captured in Windows Event Viewer — file creation detected with RuleName and process details:**

<img width="1920" height="1080" alt="Screenshot 2026-03-24 095338" src="https://github.com/user-attachments/assets/d4be8176-a982-46a6-95d4-2bf6226eb9f1" />


**MITRE ATT&CK document detail in Wazuh showing the Edge false positive — T1059.007 JavaScript, target file was an adblock snippet in Temp folder:**



- Sysmon captured process creation (Event ID 1), network connections (Event ID 3), and file drops (Event ID 11)
- The Edge false positive was firing because msedgewebview2.exe was downloading `adblock_snippet.js` to the Temp folder, which matched the T1105 pattern
- Wrote a suppression rule in local_rules.xml to filter this specific process and path combination
- Validated the fix by confirming Edge no longer triggered the alert while a real malware drop still fired correctly
- 📌 MITRE: `T1105` Ingress Tool Transfer · `T1036` Masquerading

---

### 🔴 Threat Hunting Dashboard & MITRE ATT&CK Visibility

**Wazuh Threat Hunting dashboard for Windows 10 agent — 844 total alerts, 14 high severity (Level 12+), top alert groups visible:**

<img width="1911" height="1080" alt="Screenshot 2026-03-24 115830" src="https://github.com/user-attachments/assets/4194a33f-38ef-404d-8478-e9c9137e191b" />


**Wazuh events list — PowerShell detections firing, rule 92205 (PowerShell created executable in Windows root folder) and rule 92021:**

<img width="1911" height="1080" alt="Screenshot 2026-03-24 115914" src="https://github.com/user-attachments/assets/5b5ff763-d584-4c20-9f05-0b79f9e251f4" />


**MITRE ATT&CK dashboard in Wazuh — tactic breakdown showing Impact, Privilege Escalation, Persistence, Defense Evasion, Execution, Lateral Movement, Command and Control, Discovery, Initial Access:**

<img width="1911" height="1080" alt="Screenshot 2026-03-24 115958" src="https://github.com/user-attachments/assets/d0cc2307-4b45-4486-8ad5-1f159947441b" />


- Reviewed top tactics and correlated them to specific rule IDs and alert descriptions
- Used the MITRE ATT&CK framework tab to map detections to techniques across the Kill Chain
- 📌 MITRE: `T1059` Command & Scripting Interpreter · `T1078` Valid Accounts · `T1087` Discovery

---

### 🔴 Authentication & File Integrity Monitoring

Monitored Windows and Linux authentication events and file integrity changes across both endpoints.

- Correlated Windows Event IDs 4625, 4624, and 4648 to detect failed logon sequences and flag lateral movement patterns
- Monitored Linux auth.log for sudo escalation and PAM session events on the Kali agent
- File Integrity Monitoring (FIM) detected file creation, modification, and deletion events on both Windows and Linux
- 📌 MITRE: `T1078` Valid Accounts · `T1110` Brute Force · `T1565.001` Stored Data Manipulation

---

## 📁 Files in This Repo

```
Wazuh-SIEM-SOC-Hands-On-Lab/
├── detection-rules/
│   └── local_rules.xml        (false positive suppression rule)
├── scripts/
│   └── remove-threat.sh       (active response malware deletion script)
└── config/
    └── ossec.conf-snippets    (VirusTotal integration + active response blocks)
```

---

⬅️ [Back to SOC Home Lab](https://github.com/rohithbaggu56-dot/Home-SOC-Lab-Detection-Log-Analysis) · [Back to Portfolio](https://github.com/rohithbaggu56-dot)
