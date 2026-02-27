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



