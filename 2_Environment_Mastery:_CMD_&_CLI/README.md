# Environment Mastery

---

## [1.Kali Linux Terminal (CLI)](./linux_cli_screenshots)
**1.System Discovery**
* `uname -a`
* `ls -la`
* `df -h`
* `echo`

**2.User & Identity Audit**
* `id`
* `last`
* `useradd`
* `userdel`

**3.Process & Activity Monitoring**
* `ps aux`
* `top`
* `history`

**4.Forensics & Log Analysis**
* `cat`
* `grep`
* `tail`
* `find`
* `chmod`
* `chown`

---

## [2.Windows Command Prompt (CMD)](./windows_cmd_screenshots)
**1.Identity & System**
* `whoami /all`
* `hostname`
* `systeminfo`
* `net user`
* `net localgroup administrators`

**2.Network & Connectivity**
* `ipconfig /all`
* `ping`
* `nslookup`
* `netstat -ano`
* `quser`

**3.Files & Processes**
* `tasklist /svc`
* `schtasks | more`
* `tree | more`
* `dir /ah`
* `type`

---

## 3.Key Lessons
* **Finding "Normal":** Running `ps aux` and `tasklist` helps you learn what a healthy computer looks like. This makes it easier to spot "suspicious" programs later.
* **Watching User Power:** Commands like `id` and `whoami` show who has Admin rights. This is important for stopping attackers from taking over.
* **Tracking the Path:** Tools like `history` and `last` show exactly what happened on the system. This helps you follow an intruder's footsteps.
* **Windows vs. Linux:** Even though the commands are different, the goal is always the same: gather data to keep the system safe.
