**Module:** #Network_Enumeration_with_Nmap
**Path:** `02_Toolbox/HTB_Academy/02_Network_Enumeration_with_Nmap/NSE_Scripts.md`

---

## 📂 NSE Script Categories (The 14 Pillars)

Nmap scripts are organized into categories based on their purpose and safety level.

|**Category**|**Description**|
|---|---|
|**`auth`**|Checks for authentication credentials/bypasses.|
|**`brute`**|Attempts to brute-force login credentials.|
|**`default`**|The standard scripts run with **`-sC`**. Balanced for speed/safety.|
|**`discovery`**|Actively tries to discover more info about the network (SNMP, AD, etc).|
|**`vuln`**|Scans for specific known vulnerabilities (CVEs).|
|**`exploit`**|Attempts to actively exploit a vulnerability.|
|**`dos`**|Checks for Denial of Service vulnerabilities (**Use with caution!**).|
|**`intrusive`**|High risk of crashing the service or being detected by IDS.|
|**`safe`**|Low-risk scripts that won't crash the target.|
|**`malware`**|Checks if the target is infected with known backdoors.|

---

## 🛠️ How to Execute Scripts

### 1. The "Standard" Way (Default Scripts)
```
sudo nmap <target> -sC
```

### 2. The "Categorical" Way

Runs every script within a specific category (e.g., all vulnerability scripts).
```
sudo nmap <target> --script vuln
```

### 3. The "Surgical" Way (Specific Scripts)

Run only the exact scripts you want (comma-separated).
```
sudo nmap <target> -p 25 --script banner,smtp-commands
```

### 4. The "Swiss Army Knife" (Aggressive Scan)

The **`-A`** flag is a shortcut for: Service Detection (`-sV`), OS Detection (`-O`), Traceroute, and Default Scripts (`-sC`).
```
sudo nmap <target> -A
```

---

## 🔍 Vulnerability Assessment Example

When you find a specific service (like HTTP on port 80), you can use the `vuln` category to find potential "low-hanging fruit" like CVEs or interesting directories.
```
sudo nmap 10.129.2.28 -p 80 -sV --script vuln
```

- **Useful Outputs:**
    
    - `http-enum`: Finds admin folders or readme files.
        
    - `http-wordpress-users`: Enumerates usernames.
        
    - `vulners`: Lists specific **CVE IDs** linked to the version found.

### 📝 Final Note for Section 7

**The Discovery Workflow:**

1. **Identify the Door:** `nmap --script http-enum` found the existence of `robots.txt`.
    
2. **Walk Through:** `curl` or a browser revealed the sensitive content hidden inside.
    
3. **The Lesson:** "Vulnerability" doesn't always mean a broken piece of code (CVE); sometimes it's just **misconfiguration** or leaving sensitive notes in public-facing files.