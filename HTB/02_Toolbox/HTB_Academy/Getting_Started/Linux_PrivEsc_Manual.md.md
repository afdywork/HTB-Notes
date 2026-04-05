**Category:** #Privilege_Escalation / #Post-Exploitation
**Scope:** Linux / Unix Systems

---

## 🔍 Overview

> **Logic:** Privilege Escalation is the process of moving from a Low-Privilege user (e.g., `www-data`) to a High-Privilege user (`root`). If automated tools fail, these manual checks find the "Human Errors" that scripts might miss.

---

## 🛠️ Phase 1: The "Quick Wins" (Identity)

Before searching the whole disk, check what your current user is already allowed to do.

Bash

```
whoami                      # Confirms current username
id                          # Shows Groups (check for 'lxd', 'docker', 'sudo')
sudo -l                     # THE MOST IMPORTANT: List commands you can run as root
```

---

## 🏗️ Phase 2: System Misconfigurations

Look for files that have been given "Superpowers" or directories that are "Too Open."

### 1. SUID Files (Set User ID)

Finds files that run with the owner's (usually **Root**) privileges regardless of who executes them.

Bash

```
find / -perm -4000 2>/dev/null
```

- _Next Step:_ Take the results to [GTFOBins](https://gtfobins.github.io/) to see if they can be abused.
    

### 2. World-Writable Directories

Identify where you can upload exploits, tools, or scripts without permission issues.

Bash

```
find / -writable -type d 2>/dev/null
```

---

## 🕵️ Phase 3: Information Hunting

Search the filesystem for "Hardcoded Secrets" left behind by developers or admins.

### 1. Grepping for Credentials

Search web files or configuration files for passwords.

Bash

```
# -r (recursive), -n (line number), -i (ignore case)
grep -rni "password" /var/www/html/ 2>/dev/null
grep -rni "config" /var/www/html/ 2>/dev/null
```

### 2. Hunting for SSH Keys

Check if the current user (or others) left their private keys exposed.

Bash

```
find / -name "id_rsa" 2>/dev/null
find / -name "authorized_keys" 2>/dev/null
```

---

## 📊 Escalation Checklist

|**Check**|**Command**|**Goal**|
|---|---|---|
|**Sudo**|`sudo -l`|Can I run `nmap` or `vim` as root?|
|**Cron**|`cat /etc/crontab`|Are there scheduled tasks I can overwrite?|
|**Kernel**|`uname -a`|Is the OS version old/vulnerable (e.g., DirtyPipe)?|
|**Passwd**|`cat /etc/passwd`|Can I write to this file to add a root user?|