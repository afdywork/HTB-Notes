**Category:** #Reconnaissance / #Scanning
**Scope:** Network Discovery & Vulnerability Analysis

---

## 🔍 Overview

> **Logic:** Nmap is a packet-crafting engine. It sends custom TCP/UDP packets and analyzes the response (or lack thereof) to "fingerprint" the target OS and services.

---

## ⚡ The Professional "Two-Step" Workflow

_Speed is a weapon._ Don't run heavy scans on all ports at once.

### Step 1: The "Sprayer" (Find Ports Fast)

Find every open port on the machine in under 60 seconds.
```
nmap -p- --min-rate 5000 [IP] -oN initial_ports.txt
```

- **`-p-`**: Scans all 65,535 TCP ports.
    
- **`--min-rate 5000`**: Forces a high packet rate (perfect for labs/exams).
    

### Step 2: The "Surgeon" (Deep Analysis)

Once you have the port list (e.g., 22, 80, 445), perform the heavy lifting:
```
nmap -sV -sC -p 22,80,445 [IP] -oN targeted_scan.txt
```

- **`-sV`**: Service Version detection.
    
- **`-sC`**: Runs default NSE scripts for vulnerabilities.
    

---

## 🛠️ Tactical Command Library

|**Command**|**Goal**|
|---|---|
|**`nmap [IP]`**|Basic scan of the Top 1,000 most common ports.|
|**`nmap -A [IP]`**|**Aggressive:** OS, Version, Scripts, and Traceroute.|
|**`nmap -sU [IP]`**|**UDP Scan:** Essential for finding SNMP (161) or DNS (53).|
|**`nmap --script [name] [IP]`**|Run a specific script (e.g., `smb-os-discovery.nse`).|
|**`nmap -sV --script=banner [IP]`**|Specifically target service banners for version info.|

---

## 🛡️ Firewall & IDS Evasion

> **Logic:** When the target is "Filtered," use these flags to bypass simple network defenses.

- **`-Pn`**: **No Ping.** Skip host discovery. (Crucial for Windows/Firewalled targets).
    
- **`-f`**: **Fragment Packets.** Splits packets to slip past basic deep-packet inspection.
    
- **`--source-port 53`**: Spoof traffic as if it's coming from a DNS server.
    
- **`-D RND:10`**: **Decoy.** Hide your IP among 10 random "phantom" scanners.
    

---

## 🕵️ NSE Scripting Categories

Instead of one script, run an entire category to hunt for bugs.

|**Category**|**Flag**|**Use Case**|
|---|---|---|
|**Auth**|`--script auth`|Hunt for default/weak credentials.|
|**Vuln**|`--script vuln`|Check for major CVEs (EternalBlue, etc.).|
|**Brute**|`--script brute`|Attempt password guessing on found services.|

---

## 🧪 Output Management (The "Pro" Habit)

**Always** save your results. Use `-oA [name]` to save in all three formats (Normal, XML, Grepable).
```
nmap -sS -sV -sC -Pn -p- --min-rate 5000 --reason -oA full_recon [IP]
```

- **`--reason`**: Explains _why_ a port is marked Open/Closed (e.g., received SYN/ACK).