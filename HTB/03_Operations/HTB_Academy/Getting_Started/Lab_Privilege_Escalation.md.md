**Date:** 2026-04-01 
**Category:** #PrivEsc / #Lateral_Movement / #SSH 
**Target IP:** `154.57.164.72:32047` 
**Status:** **Completed ✅**

---

## 🪜 Phase 1: Lateral Movement (user1 ➔ user2)

> **Objective:** Transition from the initial entry-point user to a user with higher or different filesystem access.

### 🛠️ Execution & Identification

1. **Initial Access:** Connected via SSH as `user1`.
    
2. **Sudo Enumeration:** Checked for "Low-Hanging Fruit" in the sudoers configuration.
    ```
    sudo -l
    ```
    
    - **Finding:** `(user2 : user2) NOPASSWD: /bin/bash`
        
    - **Logic:** This configuration allows `user1` to execute the bash shell specifically as `user2` without providing a password.
        
3. **Transition:**
    ```
    sudo -u user2 /bin/bash
    ```
    

### 🏁 Flag 1 (User)

- **Location:** `/home/user2/flag.txt`
    
- **🚩 Flag:** `HTB{l473r4l_m0v3m3n7_70_4n07h3r_u53r}`
    

---

## 🚀 Phase 2: Vertical Escalation (user2 ➔ root)

> **Objective:** Gain the highest level of administrative control (Root) by exploiting sensitive file exposure.

### 🔍 Identification: SSH Identity Theft

While enumerating as `user2`, a search for sensitive files revealed that the `/root/.ssh/` directory was world-readable—a critical security failure.

1. **Looting:** Discovered and copied the `id_rsa` (Private Key) belonging to the `root` user.
    
2. **The "Golden Rule" Fix:** Initial login attempts were rejected because the key was "too open" (Permissions `0644`).
    ```
    chmod 600 id_rsa
    ```
    
    _(This restricts the key so only the owner can read it, satisfying the SSH client's security requirements.)_
    
1. **Exploitation:**
    ```
    ssh root@154.57.164.72 -i id_rsa -p 32047
    ```
    

### 🏁 Flag 2 (Root)

- **Location:** `/root/flag.txt`
    
- **🚩 Flag:** `HTB{pr1v1l363_35c4l4710n_2_r007}`
    

---

## 🧠 Lessons Learned

- **Sudo Syntax:** The command `sudo -u [user] [command]` is the surgical tool for lateral movement. Always verify your identity with `whoami` immediately after the jump.
    
- **SSH Identity Theft:** An `id_rsa` file is a "Golden Ticket." It doesn't just give you access; it gives you a **stable, encrypted shell** that bypasses password-based logging and brute-force protections.
    
- **The "600 Rule":** SSH clients are programmed to fail if private keys are insecure. **Permissions 600** is mandatory for any stolen or generated key.
    
- **Environmental Context:** When you switch users, your `PWD` (Present Working Directory) often remains the same. Always `cd ~` or `cd /home/[new_user]` to find the relevant flags and configuration files.