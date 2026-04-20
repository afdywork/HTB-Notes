# Lab: Firewall & IDS/IPS Evasion - Medium

**Module:** #Network_Enumeration_with_Nmap
**Location:** `02_Toolbox/HTB_Academy/02_Network_Enumeration_with_Nmap/`Lab_Evasion_Medium.md`

---

## 🎯 Objective

Identify the **version** of the DNS server running on the target.

### 🧪 Theoretical Context: DNS Version Grabbing

DNS servers often store their version information in a special text record called `version.bind` in the `chaos` class. To find this, we don't just "scan" the port; we query it specifically.

---

## 🛠️ Evasion Tactics for this Lab

### 1. Switching to UDP

Since the firewall is now "stricter," it likely blocks most TCP probes. DNS primarily lives on **UDP Port 53**.

- **Command Logic:** Use `-sU` to tell Nmap to use the UDP protocol.
    

### 2. Version Detection (`-sV`)

To find the "Version" as requested by the client, we must use service version detection.

- **Command Logic:** Combine `-sU` (UDP) with `-sV` (Version detection).
    

### 3. Scripts for Specificity (`--script`)

Nmap has a specific NSE script designed to pull the version from DNS servers.

- **Script:** `dns-nsid` or querying the version record.
    

---

## 📋 Commands for Section 11

|**Command**|**Purpose**|
|---|---|
|`sudo nmap <TARGET_IP> -p 53 -sU -sV`|Scan UDP port 53 and attempt to grab the service version.|
|`sudo nmap <TARGET_IP> -p 53 -sU --script dns-nsid`|Use a specialized script to retrieve Name Server ID/Version.|
|`dig CH TXT version.bind @<TARGET_IP>`|(Non-Nmap tool) A direct DNS query to ask the server for its version.|

---

## 🔬 Step-by-Step Practical Approach

1. **The Quick Version Scan:**
    
    Start by targeting UDP port 53 specifically with version detection.
    ```
    sudo nmap <TARGET_IP> -p 53 -sU -sV -Pn -n
    ```
    
    _-Pn ensures we don't get blocked by a ping filter, and -n skips DNS resolution for speed._
    
2. **The Trusted Port Bypass (If blocked):**
    
    If the scan returns "Filtered," remember the Section 9 lesson—the firewall might only trust traffic if it **comes from** port 53.
    ```
    sudo nmap <TARGET_IP> -p 53 -sU -sV -Pn -n --source-port 53
    ```