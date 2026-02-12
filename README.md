# 🔐Wazuh SIEM – Hands-On SOC Lab (Windows & Linux)

> ⚠️ **Disclaimer**  
> This repository is created strictly for educational and defensive security learning purposes.  
> All activities were performed in a controlled lab environment. No malicious activity was conducted or encouraged.


This repository documents my hands-on work with Wazuh, focused on understanding how endpoint detection and monitoring actually work in a real SOC-style environment.

This is not a step-by-step installation guide.
Instead, it represents a practical lab where I generated real system activity (file creation, modification, deletion, authentication events), encountered detection gaps, and resolved them through configuration changes, troubleshooting, and log analysis.

The goal of this lab was to move beyond dashboards and understand why alerts appear, why they sometimes don’t, and how SOC analysts investigate those gaps.

---

## 🧪 Lab Overview

**SIEM Platform**
- Wazuh Manager & Dashboard (Ubuntu)

**Monitored Endpoints**
- Windows 10
- Kali Linux

**Monitoring Focus**
- File Integrity Monitoring (FIM)
- Authentication & PAM events
- Privilege escalation (sudo)
- Agent behavior and log visibility
- Threat Hunting vs Discover analysis

---

## ⚙️ What Was Tested in This Lab

To validate detections, I intentionally performed real actions on endpoints:

- File creation
- File modification
- File deletion
- Linux user login/logout (PAM)
- `sudo` privilege escalation
- Agent start / stop events

These actions were used to confirm whether Wazuh was:
- Collecting logs correctly
- Indexing events properly
- Triggering the expected rules and alert levels

---

## 🔧 Configuration-Level Work

A major part of this lab involved working directly with configuration files.

Key areas:
- Editing `ossec.conf`
- Enabling and tuning **syscheck (FIM)**
- Verifying monitored directories
- Restarting agents and manager services
- Validating whether config changes reflected in alerts

This helped me understand why some logs appeared in **Discover** but not in **Threat Hunting**, and how configuration directly affects visibility.

---

## 🔍 Observations & Learnings

- File deletion events triggered **high-level alerts (Level 7)** on both Linux and Windows
- Windows generated significantly more FIM events due to OS behavior
- Kali Linux showed fewer vulnerabilities compared to Windows
- No alerts does not mean no activity
- SIEM effectiveness depends more on **configuration than dashboards**

---

## 🧠 SOC Analyst Takeaways

This lab strengthened a SOC analyst mindset:

- Why is an expected alert missing?
- Is the issue with log generation, collection, or indexing?
- Is the behavior normal OS activity or suspicious?
- How do different operating systems behave under the same monitoring rules?

---

## 🧪 Lab Evidence & Observations

### 1️⃣. Active agents (Windows & Linux)
Shows active Windows and Linux agents successfully connected to the Wazuh manager.
  
  <img width="1920" height="1079" alt="Screenshot 2026-02-10 111414" src="https://github.com/user-attachments/assets/91a57329-98d1-4a25-8e4a-e364a381e734" />

### 2️⃣. Threat Hunting: Authentication, Privilege & File Activity
 shows correlated endpoint activity captured in the Threat Hunting view of Wazuh for a Kali Linux and Windows agent.

Observed events include:
- PAM login session opened and closed
- Successful sudo execution to ROOT
- File deletion detected via File Integrity Monitoring (syscheck)
- Rootcheck (host-based anomaly detection) events
- Agent start and stop events

These events appear together because Wazuh correlates authentication, privilege escalation, and file system activity by agent and time window. This reflects how SOC analysts investigate real incidents by reviewing all related actions around a single endpoint rather than isolated event types.

  
  <img width="1920" height="1080" alt="Screenshot 2026-02-10 133155" src="https://github.com/user-attachments/assets/36d08b03-a837-440e-985f-c3287915dcc7" />
  
  <img width="1920" height="1080" alt="Screenshot 2026-02-10 131410" src="https://github.com/user-attachments/assets/033d3173-dbe0-47c2-9ede-c615bbb8e42e" />

### 3️⃣. File Integrity Monitoring alerts
Shows file creation, modification, and deletion events on Windows, highlighting OS behavior differences.

  <img width="1920" height="1080" alt="Screenshot 2026-02-10 133116" src="https://github.com/user-attachments/assets/e52e8dbb-dbe4-4e9d-adb1-4b7914e24fd7" />
  
  <img width="1920" height="1080" alt="Screenshot 2026-02-10 131644" src="https://github.com/user-attachments/assets/2c4600d0-98f4-438b-8b72-2d7089248c37" />

> IP addresses are masked in screenshots as a security best practice.


---
🔗 **Navigation**  
⬅️ [Back to Portfolio](https://github.com/rohithbaggu56-dot)
---
