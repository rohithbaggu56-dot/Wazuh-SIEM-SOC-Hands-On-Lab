# 🛡️ Wazuh SIEM – SOC Monitoring & Endpoint Detection Lab

A hands-on Security Operations Center (SOC) monitoring environment built to understand how endpoint detection, firewall monitoring, and log analysis work together in a real-world defensive setup.

This lab focuses on **blue team operations**, simulating how analysts monitor endpoints behind a firewall, investigate alerts, and validate suspicious activity using Wazuh.


## 🎯 Project Summary

This project demonstrates deployment of a host-based detection system inside a controlled SOC network protected by a firewall.

The environment was designed to simulate enterprise monitoring where endpoints generate security telemetry analyzed by a centralized detection platform.

The goal was **defensive monitoring and investigation**, not penetration testing.


## 📌 Project Scope

This lab focuses on:

- Endpoint monitoring and alert detection  
- Firewall-controlled network segmentation  
- Linux & Windows log visibility  
- File Integrity Monitoring (FIM)
- Threat Hunting (TH) 
- Authentication event monitoring  
- SOC investigation workflow understanding  

---

## 🏗️ Lab Architecture

### Network Flow
<img width="1024" height="713" alt="Wazuh_lab_setup" src="https://github.com/user-attachments/assets/4f3bd58e-3a6b-48ef-82f7-7db1667ef7f6" />


### Components

**pfSense Firewall**
- Controls internal SOC network traffic
- Provides network segmentation
- Simulates enterprise perimeter defense

**Wazuh Server**
- Centralized log collection
- Alert correlation engine
- Endpoint monitoring platform

**Endpoints**
- Windows VM generating security events
- Linux systems producing authentication logs
- Kali Linux used for activity simulation


## ⚙️ Wazuh Implementation

The Wazuh platform was deployed to monitor endpoint behavior and detect suspicious system activities.

### Configured Features

- Agent enrollment and monitoring
- File Integrity Monitoring (FIM)
- Threat Hunting (TH)
- Authentication log analysis
- Privilege escalation monitoring
- Security event alerting

Configuration tuning performed using:

- `ossec.conf`
- Agent configuration policies
- Log source adjustments


## 🧪 Activity Simulation & Detection Testing

To validate detection visibility, multiple system activities were performed:

- File creation and deletion events
- Unauthorized file modification attempts
- Linux sudo privilege escalation
- Authentication attempts monitoring
- Agent communication verification

These actions generated alerts analyzed from an analyst perspective.


## 🔎 SOC Investigation Approach

Each alert was reviewed using a structured workflow:

1. Identify triggered rule
2. Review event source
3. Validate user activity
4. Analyze timestamps and behavior
5. Determine normal vs suspicious activity
6. Document findings

This helped understand **why alerts exist**, not just how they appear.


## 🧭 Detection Visibility Learned

The lab helped demonstrate how:

- Endpoint logs reveal attacker behavior
- Privilege escalation attempts appear in telemetry
- File integrity monitoring detects unauthorized changes
- Authentication anomalies become investigation starting points


## 🧠 SOC Analyst Skills Practiced

- Endpoint detection monitoring
- Alert triage and validation
- Linux & Windows log investigation
- Detection rule understanding
- Security event correlation
- Defensive analysis mindset

---
## 📸 Lab Screenshots

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


---

## 🧠 Key Takeaways

- Learned how endpoint detection works inside a monitored SOC network rather than as a standalone tool.
- Understood how firewall segmentation (pfSense) improves visibility and security monitoring.
- Gained practical experience analyzing Linux and Windows security events from an analyst perspective.
- Observed how attacker behavior appears through authentication logs, file changes, and privilege escalation events.
- Developed alert triage skills by validating whether alerts represent normal activity or suspicious behavior.
- Understood the importance of centralized logging for incident investigation and detection workflows.
- Improved understanding of how SOC analysts investigate alerts step-by-step instead of relying only on tools.
- Learned how detection rules help identify abnormal system behavior early in the attack lifecycle.

---
🔗 **Navigation**  
⬅️ [Back to SOC-Home-Lab-BlueTeam](https://github.com/rohithbaggu56-dot/SOC-Home-Lab-BlueTeam))
---
