# 🗃️ SecLists & Discovery Commands

_The ultimate collection of wordlists paired with the tools that use them._

## 📍 Locations

- **Pwnbox/Kali:** `/usr/share/seclists/`
    
- **GitHub:** `https://github.com/danielmiessler/SecLists`
    

---

## 🚀 1. Web Directory Discovery (The "Hidden Folder" Finder)

**Tool:** `gobuster` **Wordlist:** `/usr/share/seclists/Discovery/Web-Content/common.txt`

### Standard Directory Scan

> Use this when you find a web server (Port 80/443) and want to find hidden folders like `/admin` or `/config`.

Bash

```
gobuster dir -u http://[TARGET_IP]/ -w /usr/share/seclists/Discovery/Web-Content/common.txt
```

### Searching for Specific File Types

> Use the `-x` flag to look for specific extensions (like `.php`, `.xml`, or `.txt`).

Bash

```
gobuster dir -u http://[TARGET_IP]/ -w /usr/share/seclists/Discovery/Web-Content/common.txt -x php,xml,txt
```

---

## 🚀 2. DNS & Subdomain Discovery

**Tool:** `gobuster dns` or `ffuf` **Wordlist:** `/usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt`

### Finding Subdomains

> Use this if the target is a domain name (e.g., `nibbles.htb`) rather than just an IP.

Bash

```
gobuster dns -d [TARGET_DOMAIN] -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt
```

---

## 🚀 3. Password Cracking (The "King" List)

**Tool:** `hydra` (for logins) or `hashcat` (for hashes) **Wordlist:** `/usr/share/seclists/Passwords/Leaked-Databases/rockyou.txt.tar.gz`

### Brute Forcing a Login (e.g., SSH)

> **Warning:** Only use this if there is no account lockout protection!

Bash

```
hydra -l admin -P /usr/share/seclists/Passwords/Leaked-Databases/rockyou.txt ssh://[TARGET_IP]
```

---

## 💡 Pro-Tips & Helper Commands

### 🔍 Find a list quickly

Bash

```
locate seclists | grep [keyword]
```

### 📏 Check list size (How many guesses?)

Bash

```
wc -l /usr/share/seclists/Discovery/Web-Content/common.txt
```

### 🧊 Unzipping RockYou (Required on new Kali installs)

_RockYou is often compressed to save space. You must unzip it before using it._

Bash

```
sudo gunzip /usr/share/seclists/Passwords/Leaked-Databases/rockyou.txt.gz
```