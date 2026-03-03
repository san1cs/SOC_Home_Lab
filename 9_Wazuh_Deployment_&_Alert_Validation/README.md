# Wazuh Deployment & Alert Validation

---

## 1.Wazuh HIDS Deployment and OVA Integration

### 1.1.Virtual Box Config

* **Wazuh:** https://documentation.wazuh.com/current/deployment-options/virtual-machine/virtual-machine.html

* **Network:** eth0 "Host-only adapter"

* **Network:** eth1 "Nat network"

*  _**Uncheck cable enable**, wazuh need working internet connection. Huge database for Wazuh-manager is not required in small lab environment._

*  **RAM Allocation:** 8GB

### 1.2.Wazuh Setup: Starting Services and Configuring the Manager

* **User name:** `wazuh-user`

* **Password:** `wazuh`

* `sudo systemctl status wazuh-indexer wazuh-manager wazuh-dashboard`

* `sudo systemctl restart wazuh-indexer`

* `sudo systemctl restart wazuh-manager`

* `sudo systemctl restart wazuh-dashboard`

![wazuh](./screenshots/wazuh1.png)

### 1.3.Multi-Platform Agent Enrollment: Onboarding Windows and Kali Linux Endpoints

* **Deploy new agent**

* **Linux** & **Windows** `DEB amd64` & `MSI 32/64 bits`

* **Assign a server address:** `192.168.251.13` _(Wazuh IP)_

* _Install the wazuh agent in the system_

* **Linux:** `sudo systemctl daemon-reload && sudo systemctl enable wazuh-agent && sudo systemctl start wazuh-agent` _(Bash Shell)_

* **Windows:** `WIN START Wazuh` _(Powershell)_

![wazuh](./screenshots/wazuh2.png)

---

## 2.Login Alerts

### 2.1.False Positive: Multiple Login Failed SSH

**1.Setting up the SSH in Kali Linux**

* `sudo apt install openssh-server -y`

* `sudo systemctl enable --now ssh`

**2.Login via SSH through Windows**

* **CMD:** `ssh kali@192.168.251.3`

![win](./screenshots/win1.png)

**3.Alert Triggered on SIEM Dashboard**

![wazuh](./screenshots/wazuh3.png)

**4.Looking up the events to veriy the alert**

![wazuh](./screenshots/wazuh4.png)

* The sequence of five authentication failures followed immediately by a successful login validates this activity as a false positive, characterizing routine user error rather than a malicious brute-force attempt.

* [**False Positive**](./report/events-2026-02-27T10_05_22.351Z.csv)

### 2.3.True positive: RDP Dictionary attack

**1.Setting up the RDP in Windows**

* **Turn on the RDP connection in windows:** Settings > System > Remote Desktop

* _For the lab demonstration "Require computers to use Network Level Authentication (NLA) to connect" has been turned off_

**2.Making sure the windows to collect the log**

* **Powershell:** `auditpol /set /subcategory:"Logon" /success:enable /failure:enable`

* _By default, Windows is often "quiet" it might not log every failed password attempt. This command forces it to speak up so that Wazuh Agent can "hear" what is happening and report it to dashboard._

**3.Running a Dictionary attack using hydra**

* `sudo hydra -l Bob -P /usr/share/wordlists/rockyou.txt -t 1 192.168.251.11 rdp -vV`

**4.Checking the alert on SIEM Dashboard**

![wazuh](./screenshots/wazuh5.png)

![wazuh](./screenshots/wazuh6.png)

* The presence of a final logon failure (Rule 60122) just 4 milliseconds after the account lockout event (Rule 60115) confirms an automated brute-force attack, as the attacker's parallel threads continued to hit the service even after the security policy had successfully locked the account."

* [**True Positive**](./report/events-2026-02-28T07_42_37.205Z.csv)

### 2.4.Suspicious User Login: Insider Threat

* **Standard user:** Alice logged in outside of the office hour

* Executed powershell command with admin rights `wevtutil cl System`

![wazuh](./screenshots/wazuh7.png)

* A successful Privilege Escalation and Obfuscation attempt was identified; by using administrative credentials to execute wevtutil, the insider (Alice) attempted to delete system audit trails—an action immediately flagged by Wazuh as a high-severity tactical alert.

* [**Insider Threat**](./report/events-2026-02-28T12_51_08.497Z.csv)

---

## 3.Real-time Detection of Unauthorized File Modifications (FIM)

### 3.1.Configuring the "Watchdog"

* **CMD Admin:** `notepad "C:\Program Files (x86)\ossec-agent\ossec.conf"`

* In`<syscheck>` section

* **Adding Specific File:** `<directories realtime="yes" report_changes="yes" check_all="yes" restrict="Payment.txt">C:\Users\Bob\Documents\Reports</directories>`

* **win + r** > Run > services.msc > Wazuh > Restart

![win](./screenshots/win2.png)

* _**Linux:** `sudo nano /var/ossec/etc/ossec.conf`_

* _**Linux:** `sudo systemctl restart wazuh-agent`_
 
### 3.2.The "Detection" Simulation

* C:\Users\Bob\Documents\Reports\ [**Payment.txt**](./folder/Payment.txt)

* Changing the values & Deleting the file

* File Modifications (FIM) events detection in Wazuh

![wazuh](./screenshots/wazuh8.png)

![wazuh](./screenshots/wazuh9.png)

* To mitigate the risk of unauthorized data modification, Wazuh's File Integrity Monitoring (FIM) was configured with `realtime="yes"` and `report_changes="yes"`. This setup allowed instantly detect when a file was created or deleted by a user. Most importantly, by capturing the 'diff' of file modifications, the system provided solid proof of the specific data altered inside the files, transforming a simple 'modification' alert into a deep-dive forensic investigation tool."

* [**FIM**](./report/events-2026-03-02T02_20_40.511Z.csv)

---

## 4.Vulnerability Detection

### 4.1.Vulnerability Assessment Report

* The vulnerability scan performed by the Wazuh Cyber Threat Intelligence (CTI) module identified a critical risk level on the target machine. The system is currently exposed to a vast attack surface due to outdated operating system components and browser software.

![wazuh](./screenshots/wazuh10.png)

* The vulnerability assessment of DESKTOP-PBJ85VH revealed a high-risk security posture with 17 Critical vulnerabilities. The primary attack vector identified is an outdated Windows 10 build (19045.3803) and an obsolete version of Microsoft Edge.

![wazuh](./screenshots/wazuh11.png)

* [**Critical**](./report/events-2026-03-03T02_45_31.573Z.csv)

* **Remediation Plan:** A comprehensive Windows Update would resolve over 90% of these findings by patching the kernel and updating system-critical browser components. This demonstrates that 'Unpatched Software' was the primary reason the simulated malware was able to operate with such high privileges.

### 4.2.Remediation (The "Fix")

* **OS Patching:** Execute a full Windows Update to move the build from 19045 to the latest 2026 security baseline. This will resolve approximately 12 of the 17 Critical CVEs.

* **Application Lifecycle:** Manually update or reinstall Microsoft Edge to version 130+

 ![wazuh](./screenshots/wazuh12.png)

---

