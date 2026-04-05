# 🏁 Lab: Web Enumeration (Section 8)
**Target IP:** `154.57.164.74:30517`
**Date:** 2026-03-23
**Status:** Completed ✅

## 🛠️ Execution Flow
1. **Directory Discovery:** Ran `gobuster` using `common.txt`.
   - Command: `gobuster dir -u http://154.57.164.74:30517 -w /usr/share/seclists/Discovery/Web-Content/common.txt -t 50`
   - **Key Finding:** Identified `/robots.txt` (Status: 200).

2. **Manual Inspection (Robots):** Checked `http://[IP]:[PORT]/robots.txt`.
   - **Finding:** Disallow entry for `/admin-login-page.php`.

3. **Source Code Analysis:** Navigated to the hidden admin page and viewed source (`CTRL+U`).
   - **Finding:** Hardcoded credentials in HTML comments: ``.

4. **Exploitation:** Logged in using discovered credentials to retrieve the flag.

## 🚩 Flag
`HTB{w3b_3num3r4710n_r3v34l5_53cr375}`

## 🧠 Lessons Learned
- **Wordlist Selection:** DNS lists are too large for directory scans. Use `common.txt` for speed/accuracy.
- **The "Robots" Map:** Never ignore `robots.txt`; it often points directly to what the admin is trying to protect.
- **Comment Leaks:** Developers often leave "temporary" credentials in code. Always `CTRL+U`.