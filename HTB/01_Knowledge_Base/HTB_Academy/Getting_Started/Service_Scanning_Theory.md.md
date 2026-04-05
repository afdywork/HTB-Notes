# 🛰️ Service Scanning & Enumeration Theory

## 📂 Key Concepts

- **Service:** An application running on a server that performs functions for users (e.g., Web Server, File Share).
    
- **Ports:** Ranges from 1 to 65,535.
    
- **Well-known Ports:** 1-1,023 (Reserved for privileged services like HTTP, SSH, FTP).
    
- **Port 0:** Reserved/Wildcard; services usually bind to the next available port above 1,024.
    

## 📡 Scanning States

- **Open:** The service is actively listening and accepting connections.
    
- **Filtered:** A firewall or network filter is blocking the probes; Nmap cannot tell if the port is open or closed.
    
- **Closed:** The port is reachable but no service is listening on it.
    

## 🚩 Common Service Indicators

|**Port**|**Common Service**|**Likely OS Target**|**Notes**|
|---|---|---|---|
|21|FTP|Any|Check for **Anonymous Login**.|
|22|SSH|Linux / Unix|Check version for **User Enumeration** bugs.|
|80 / 443|HTTP / HTTPS|Any|Check **Page Source (CTRL+U)** for plugins.|
|139 / 445|SMB / Samba|Windows / Linux|Use `smbclient` to list shares.|
|3389|RDP|Windows|Common target for **BlueKeep** or Brute Force.|

---

## 🕵️ Advanced Enumeration: The "Detective" Mindset

When automated scanners (Nmap/Gobuster) don't give you a direct exploit, you must pivot to manual inspection.

## 💡 The Enumeration Golden Rule

> **"If the scanners aren't talking, start reading."**

**Manual Check-list for Web Applications:**

1. **Page Source (CTRL+U):** Search for `plugins`, `themes`, or `version` strings.
    
2. **Sensitive Files:** Manually check for `/robots.txt`, `/sitemap.xml`, or `/.git/`.
    
3. **Visual Clues:** Look at "Recent Posts," "Comments," or "Footer" text for software names and version numbers.
    
4. **Directory Structure:** Standard CMS paths (like `/wp-content/plugins/` for WordPress) often leak information even if directory listing is disabled.
    

---

## 🚀 Exploitation Logic: Bridging the Gap

The goal of enumeration is to find a **Product Name** and a **Version Number**. This allows you to perform a **Targeted Strike**.

## The Workflow

1. **Identify:** Use Nmap to find the service and version (`-sV`).
    
2. **Research:** Use `searchsploit` or Google to find public exploits (CVEs).
    
3. **Verify:** Check if the vulnerable component actually exists (e.g., use a browser to see if a plugin folder is reachable).
    
4. **Execute:** Use a tool like **Metasploit** to deliver the payload.
    

## Vulnerability Types encountered:

- **Remote File Read / Path Traversal:** Exploiting a bug to read files outside the web root (e.g., `/etc/passwd` or `/flag.txt`) using `../` sequences.
    
- **Remote Code Execution (RCE):** The ultimate goal; gaining a shell to run commands directly on the target system.