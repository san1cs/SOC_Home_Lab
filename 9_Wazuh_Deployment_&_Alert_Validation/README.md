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

**Setting up the SSH in Kali Linux**

* `sudo apt install openssh-server -y`

* `sudo systemctl enable --now ssh`

**Login via SSH through Windows**

* **CMD:** `ssh kali@192.168.251.3`

![win](./screenshots/win1.png)

**Alert Triggered on SIEM Dashboard**

![wazuh](./screenshots/wazuh3.png)

**Looking up the events to veriy the alert**

![wazuh](./screenshots/wazuh4.png)

* The sequence of five authentication failures followed immediately by a successful login validates this activity as a false positive, characterizing routine user error rather than a malicious brute-force attempt.

* [**False Positive Report**](./report/events-2026-02-27T10_05_22.351Z.csv) _(.cvs file)_

### 2.3.True positive: RDP Brute Force

### 2.4.Suspicious User Login: Insider Threat

---



