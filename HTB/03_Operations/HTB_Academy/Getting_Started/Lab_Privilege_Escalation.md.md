# 🏁 Lab: Privilege Escalation (Section 11)
**Target IP:** 154.57.164.72:32047
**Date:** 2026-04-01
**Status:** Completed ✅

## 🛠️ Execution Flow

### 1. Initial Access & Identification
* **Action:** Logged in via SSH as `user1`.
* **Command:** `ssh user1@154.57.164.72 -p 32047`
* **Enumeration:** Checked sudo privileges.
* **Command:** `sudo -l`
* **Finding:** `(user2 : user2) NOPASSWD: /bin/bash`. This indicates `user1` can "become" `user2` without a password.

### 2. Lateral Movement (user1 ➔ user2)
* **Action:** Spawned a new bash shell as `user2`.
* **Command:** `sudo -u user2 /bin/bash`
* **Verification:** `whoami` confirmed identity as `user2`.
* **Flag 1:** Located in `/home/user2/flag.txt`.
* **🚩 Flag:** `HTB{l473r4l_m0v3m3n7_70_4n07h3r_u53r}`

### 3. Vertical Privilege Escalation (user2 ➔ root)
* **Enumeration:** Browsed filesystem for sensitive files. Discovered `/root/.ssh/` was world-readable.
* **Finding:** Found the root user's private SSH key (`id_rsa`).
* **Exfiltration:** Copied the contents of `id_rsa` to the local attack machine.
* **Troubleshooting:** Initial SSH attempt failed due to `Permissions 0644`. 
* **Fix:** Restricted permissions on the stolen key.
* **Command:** `chmod 600 id_rsa`
* **Exploitation:** Logged in directly as root using the stolen identity.
* **Command:** `ssh root@154.57.164.72 -i id_rsa -p 32047`

### 4. Root Access
* **Verification:** `whoami` confirmed identity as `root`.
* **Flag 2:** Located in `/root/flag.txt`.
* **🚩 Flag:** `HTB{pr1v1l363_35c4l4710n_2_r007}`

## 🧠 Lessons Learned
1. **Sudo Syntax:** `sudo -u [user] [command]` is the key for lateral movement. Adding trailing slashes or extra arguments can break the `NOPASSWD` rule.
2. **SSH Identity Theft:** Finding an `id_rsa` is a "Golden Ticket." It allows you to bypass password authentication entirely by "borrowing" a user's digital identity.
3. **The 600 Rule:** SSH clients strictly enforce private key security. If a key is readable by "others" (`0644`), it will be ignored. Always `chmod 600`.
4. **Environment Context:** Switching users often keeps you in the old user's directory. Always `cd ~` or `cd /home/[user]` after switching to find relevant files.