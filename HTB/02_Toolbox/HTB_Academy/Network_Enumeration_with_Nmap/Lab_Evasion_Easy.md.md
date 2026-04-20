# Firewall & IDS/IPS Evasion - Easy

**Module:** #Network_Enumeration_with_Nmap
**Location:** `02_Toolbox/HTB_Academy/02_Network_Enumeration_with_Nmap/`Lab_Evasion_Easy.md`

---

## 🎯 Objective

Identify the **Operating System (OS)** of the target machine without triggering enough alerts to get banned.

### 🔍 Monitoring Tools

- **Status Page:** `http://<TARGET_IP>/status.php`
    
- **Goal:** Keep the alert count below the "Ban" threshold while gathering OS fingerprints.
    

---

## 🛠️ Potential Strategies

### 1. The "Gentle" OS Scan

A standard `-O` scan is very noisy because it sends multiple types of crafted packets to see how the stack responds. To stay quiet, we should combine it with timing and evasion.
```
# Option A: Slower timing to avoid rate-limit alerts
sudo nmap <TARGET_IP> -O -T2 -Pn

# Option B: Using a specific source port (DNS bypass)
sudo nmap <TARGET_IP> -O --source-port 53 -Pn
```

### 2. The "Decoy" Strategy

If the IDS is looking for a single IP doing too much, hide your real IP among decoys.
```
sudo nmap <TARGET_IP> -O -D RND:10 -Pn
```

### 3. Service Versioning (Alternative OS Detection)

Sometimes you can guess the OS by looking at the versions of services running (e.g., specific IIS versions point to Windows, specific Apache/OpenSSH builds point to Linux/Ubuntu).
```
# Scan a few specific ports instead of all of them
sudo nmap <TARGET_IP> -p 22,80,443 -sV -Pn
```

---

## 📋 Commands Used in this Section

|**Command**|**Purpose**|
|---|---|
|`-O`|Enable OS Detection.|
|`-Pn`|Disable ICMP (Ping) to stay quiet against firewalls.|
|`-T2` / `-T3`|Lower the timing template to avoid "Aggressive Scan" signatures.|
|`--source-port 53`|Try to spoof traffic coming from a "Trusted" DNS port.|
|`-D RND:X`|Use X number of random decoys to mask your IP.|

**Alternative OS Discovery:**

- **Tool:** `curl -I http://<IP>`
    
- **Technique:** Inspecting the `Server:` header.
    
- **Advantage:** Very quiet. It is a standard HTTP request that almost never triggers an IDS/IPS alert, whereas an Nmap `-O` scan triggers dozens.