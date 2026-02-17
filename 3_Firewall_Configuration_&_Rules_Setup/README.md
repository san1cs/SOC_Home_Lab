# Firewall Configuration and Rule Setup
---
## 1.1.Nmap scan on metasploitable 2

* **Action:** Performed a 'Full Port Discovery' scan. This allowed to map out all active services on the Metasploitable VM. By identifying open ports like 21 (FTP) and 80 (HTTP)

* **Result:** [Nmap report for metasploitable port scan](./scan_reports/nmap_meta_scan.txt)

* **Lesson:** Without a firewall, any user on the network can see every service running on the server. This proves that a firewall is the first and most important layer of defense to hide sensitive ports.

---

 ## 2.1.Windows without Firewall

* **Action:** Disabling the Windows Firewall to demonstrate how an unprotected host exposes its services and increases the attack surface during an Nmap discovery scan.

![windows firewall](./screenshots/windows_nfirewall.png)

* **Result:** [Nmap report for windows prot scan](./scan_reports/nmap_win_scan.txt)

## 2.2.Windows with Firewall

* **Action:** Enabling the Windows Firewall to demonstrate how active filtering controls port visibility and blocks unauthorized Nmap discovery scans.

![windows firewall](./screenshots/windows_firewall.png)

* **Result:** [Nmap report for windows prot scan](./scan_reports/nmap_win_fscan.txt)

---

## 3.1.
