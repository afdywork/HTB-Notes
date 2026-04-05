**Category:** #Enumeration / #Exploitation
**Service Port:** `21` (TCP)

---

## 🔍 Overview

> **Logic:** FTP is a standard network protocol used to transfer computer files between a client and a server. It is **insecure by default**, sending all data (including passwords) in cleartext.

---

## 🛠️ Essential Commands

### 1. Basic Connection

Standard way to log in if you have credentials:
```
# -p: Passive mode (often required to bypass firewalls)
ftp -p [Target_IP]
```

### 🚩 The "Anonymous" Login

The first thing you should try on any FTP port. Many admins leave this enabled for "convenience."

- **Username:** `anonymous`
    
- **Password:** _(Leave blank or use a fake email like `guest@guest.com`)_
    

---

## 🎮 Inside the FTP Prompt

Once the connection is established (indicated by a `ftp>` prompt), use these commands to navigate:

|**Command**|**Action**|
|---|---|
|**`ls`**|List files and folders in the current directory.|
|**`cd [dir]`**|Move into a specific folder.|
|**`type binary`**|**Critical:** Switch to Binary mode before downloading non-text files (images, zip, exe).|
|**`get [file]`**|**Download** a file to your local machine.|
|**`put [file]`**|**Upload** a file (if you have Write permissions).|
|**`exit`** or **`bye`**|Close the session.|

---

## 🚀 Pro-Tip: Nmap Automation

Before manual interaction, use Nmap to check for common FTP vulnerabilities automatically:
```
nmap -sV -sC -p 21 [Target_IP]
```

_Look for the script output: `ftp-anon: Anonymous FTP login allowed`._