**Date:** 2026-03-23 
**Category:** #Web / #Enumeration 
**Target IP:** `154.57.164.74:30517` 
**Status:** **Completed ✅**

---

## 🔍 Investigation Phase

### 1. Automated Directory Discovery

> **Logic:** Using a wordlist to "fuzz" the web server for hidden directories that aren't linked on the main page.

**Command:**
```
gobuster dir -u http://154.57.164.74:30517 -w /usr/share/seclists/Discovery/Web-Content/common.txt -t 50
```

- **Key Finding:** Identified `/robots.txt` (Status: 200).
    

---

### 2. Manual Inspection (`robots.txt`)

Checked `http://154.57.164.74:30517/robots.txt` to see what the administrator is attempting to hide from search engines.

- **Finding:** `Disallow: /admin-login-page.php`
    

---

### 3. Source Code Analysis (The "Gold Mine")

Navigated to the hidden `/admin-login-page.php` and performed a manual source code review (`CTRL + U`).

- **Finding:** Hardcoded credentials discovered inside HTML comments: ``
    

---

## 🏁 Final Objective: Exploitation

Logged into the administrative portal using the leaked credentials and successfully retrieved the flag.

> **🚩 Flag:** `HTB{w3b_3num3r4710n_r3v34l5_53cr375}`

---

## 🧠 Lessons Learned

- **Wordlist Selection:** Size matters. DNS lists are overkill for directory scans. Use `common.txt` or `directory-list-2.3-medium.txt` for the best balance of speed and coverage.
    
- **The "Robots" Map:** `robots.txt` isn't a security feature; it’s a roadmap for attackers. It literally tells you where the sensitive files are located.
    
- **Comment Leaks:** "Temporary" fixes are often permanent security holes. Developers frequently forget to remove debugging credentials from the frontend code.