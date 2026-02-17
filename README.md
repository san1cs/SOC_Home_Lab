# SOC Home Lab: End-to-End Security Operations & Monitoring

## Project Overview
This project is an end-to-end Security Operations Center (SOC) Home Lab designed to simulate a real-world enterprise environment. It demonstrates proficiency in SIEM deployment (ELK/Wazuh), network traffic analysis, incident response, threat detection, alert validation, firewall configuration, remediation and vulnerability management.

**Objective:** To establish a controlled environment for observing adversary tactics, techniques, and procedures (TTPs) while validating the effectiveness of industry-standard detection and response tools.

## Table of Contents
1. [Lab Setup](./01_Lab_Setup/)
2. [Environment Mastery: CMD & CLI](./02_Environment_Mastery:_CMD&_&_CLI/)
3. [Firewall Configuration & Rules Setup](./03_Firewall_Configuration_&_Rules_Setup/)
4. [Log Analysis: Linux & Windows](./04_Log_Analysis:_Linux_&_Windows/)
5. [Phishing Simulation](./05_Phishing_Simulation/)
6. [Threat Detection & Incident Response](./06_Threat_Detection_&_Incident_Response/)
7. [ELK Deployment & Troubleshooting ](./07_ELK_Deployment_&_Troubleshooting/)
8. [Wireshark Packet Analysis](./08_Wireshark_Analysis/)
9. [Wazuh Deployment & Alert Validations](./09_Wazuh_Deployment_&_Alert_Validation/)
10. [Vulnerability Management & Reports](./10_Vulnerability_Management_&_Reports/)

---

## Technical Stack & Tools

* **SIEM / XDR:** Wazuh (Manager & Agents), ELK Stack (Elasticsearch, Logstash, Kibana)
* **Network Analysis:** Wireshark, Nmap
* **Threat Intelligence:** VirusTotal
* **Endpoint Security:** Windows Defender (NGAV), Wazuh EDR
* **Host-Based Analysis:** Windows Event Viewer, Process Explorer
* **Social Engineering:** Social Engineering Toolkit (SET), Zphisher
* **Offensive Security:** Kali Linux, Metasploit Framework
* **Vulnerability Management:** OpenVAS (GVM)

## Skills Demonstrated

* **SIEM & Monitoring:** Setting up Wazuh and ELK Stack for centralized logging and real-time alert monitoring.
* **Threat Detection:** Identifying malicious patterns and checking file hashes on VirusTotal to confirm threats.
* **Incident Response:** Executing the full IR lifecycle, including alert validation, containment, and remediation.
* **Log Analysis:** Deep-dive analysis of Windows Event Logs and Linux Syslogs (auth.log, syslog).
* **Network Security:** Traffic analysis using Wireshark and configuring Firewall rules to enforce network segmentation.
* **System Administration:** Hardening and managing both Windows and Linux (Kali) environments

---

## Disclaimer
_All security testing and simulations performed in this project were conducted in a closed, isolated, and authorized home lab environment. This project is for educational purposes only. Any tools or techniques described should never be used on systems or networks without explicit permission._
