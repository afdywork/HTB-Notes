**Category:** #Reconnaissance / #Enumeration
**Phase:** Pre-Exploitation

---

## 📂 Fundamental Concepts

> **Logic:** Every open port is a potential doorway. Enumeration is the process of peering through the keyhole to see what's inside before trying to kick the door down.

### 🔢 Port Anatomy

- **Well-known Ports (1-1,023):** Reserved for privileged services (HTTP, SSH, FTP). Requires root/admin to bind.
    
- **Registered Ports (1,024-49,151):** Used by specific applications (e.g., MySQL on 3306).
    
- **Port 0:** Reserved/Wildcard. Services bind to the next available port > 1,024.
    

### 📡 Scanning States (The "Nmap Verdict")

|**State**|**Meaning**|**Action**|
|---|---|---|
|**Open**|Service is actively listening.|**Enumerate Version!**|
|**Filtered**|Firewall/IDS is dropping packets.|Try fragmentation or different source ports.|
|**Closed**|Reachable, but no service is home.|Move to the next port.|

---

## 🚩 Common Service Indicators

|**Port**|**Service**|**Target OS**|**Tactical Notes**|
|---|---|---|---|
|**21**|FTP|Any|Check for `Anonymous` login.|
|**22**|SSH|Linux|Check version for User Enumeration bugs.|
|**80/443**|HTTP(S)|Any|Check Page Source (`CTRL+U`) for plugins/versions.|
|**139/445**|SMB|Win/Linux|Use `smbclient -L` to list shares.|
|**3389**|RDP|Windows|Check for BlueKeep or Brute Force potential.|

---

## 🕵️ The "Detective" Mindset

> **Golden Rule:** "If the scanners aren't talking, start reading."

### 📝 Manual Web Checklist

- [ ] **Page Source (`CTRL+U`):** Search for `v=`, `version`, or unique plugin names.
    
- [ ] **Hidden Files:** Check `/robots.txt`, `/sitemap.xml`, or `/.git/`.
    
- [ ] **Visual Clues:** Look at "Recent Posts" or "Footer" for CMS names (WordPress, Joomla).
    
- [ ] **Directory Structure:** Fuzz for `/wp-content/`, `/admin/`, or `/config/`.
    

---

## 🚀 Exploitation Logic: The Targeted Strike

The goal of enumeration is to find a **Product Name** + **Version Number**.

### 🔄 The Workflow

1. **Identify:** Use Nmap `-sV` to get the version string.
    
2. **Research:** Use `searchsploit` or Google the CVE.
    
3. **Verify:** Manually check if the vulnerable path/file exists.
    
4. **Execute:** Use Metasploit or a standalone PoC script.
    

### 💀 Common Vulnerability Types

- **LFI / Path Traversal:** Using `../` to read `/etc/passwd`.
    
- **RCE (Remote Code Execution):** The "Holy Grail." Gaining a shell to run commands directly.