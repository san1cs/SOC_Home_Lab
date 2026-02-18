# Firewall Configuration and Rule Setup
---
## 1.Nmap scan on metasploitable 2

* **Action:** Performed a 'Full Port Discovery' scan. This allowed to map out all active services on the Metasploitable VM. By identifying open ports like 21 (FTP) and 80 (HTTP)

* **Result:** [Nmap report for metasploitable port scan](./scan_reports/nmap_meta_scan.txt)

* **Lesson:** Without a firewall, any user on the network can see every service running on the server. This proves that a firewall is the first and most important layer of defense to hide sensitive ports.

---

 ## 2.Windows Defender Firewall

 ### 2.1.Without Firewall

* **Action:** Disabling the Windows Firewall to demonstrate how an unprotected host exposes its services and increases the attack surface during an Nmap discovery scan.

![windows firewall](./screenshots/windows_nfirewall.png)

* **Result:** [Nmap report for windows prot scan](./scan_reports/nmap_win_scan.txt)

### 2.2.With Firewall

* **Action:** Enabling the Windows Firewall to demonstrate how active filtering controls port visibility and blocks unauthorized Nmap discovery scans.

![windows firewall](./screenshots/windows_firewall.png)

* **Result:** [Nmap report for windows prot scan](./scan_reports/nmap_win_fscan.txt)

---

## 3.Linux ufw Firewall

### 3.1.Setting up the ufw

* `ufw status`
* `ufw enable`  
* `sudo ufw default deny incoming`
* `sudo ufw default allow outgoing`

### 3.2.Custom rules (Inbuoud/Outboud)

* `sudo ufw allow ssh`
* `sudo ufw deny 80/tcp`
* `sudo ufw deny from 192.168.251.5`
* `sudo ufw reject out to 57.144.48.1`

## 4.Outbound custom rules

### 4.1.Blocking the HTTP

**Action:** Executed the command `sudo ufw deny 80/tcp` to block on all unencrypted outbound HTTP traffic.

![kali](./screenshots/kali80.png)

**Result:** The firewall successfully blocked the http connection

### 4.2.Blocking the IP

**Action:** Executed the command `sudo ufw reject out to 192.168.251.5` to block the icmp from the metasploitable 2.

![meta](./screenshots/meta.png)

**Result:** The firewall successfully blocked the icmp request

---

## 5.Windows
