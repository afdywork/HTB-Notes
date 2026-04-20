# Nmap: Firewall & IDS/IPS Evasion

**Module:** #Network_Enumeration_with_Nmap
**Location:** `02_Toolbox/HTB_Academy/02_Network_Enumeration_with_Nmap/`Nmap_Evasion_Techniques.md`

---

## 🧱 Understanding the Obstacles

### Firewalls

A firewall filters traffic based on rules.

- **Dropped Packets:** The firewall ignores the request. Nmap shows this as `filtered`.
    
- **Rejected Packets:** The firewall sends an error back (e.g., ICMP Port Unreachable or TCP RST).
    

### IDS/IPS

- **IDS (Detection):** Passive. It watches and logs "suspicious" patterns (like a rapid port scan).
    
- **IPS (Prevention):** Active. It drops your connection or **blocks your IP address** if it detects a signature.
    

---

## 🎭 Evasion Techniques

### 1. ACK Scan (`-sA`) - Mapping Firewall Rules

Regular SYN scans are often blocked. However, many firewalls allow **ACK** packets through because they assume the connection was already established from the _inside_.

- **Purpose:** To map out firewall rules, not to find open ports.
    
- **Result:** If a port is `unfiltered`, it means the firewall is letting ACK packets through.
    

### 2. Decoys (`-D`) - Hiding in a Crowd

You can blend your IP address with a list of random or specific "Decoy" IPs. The target's logs will show 5-10 different IPs scanning them at the same time, making it hard to tell which one is the real attacker.

- **Command:** `sudo nmap <IP> -D RND:5` (Generates 5 random decoys).
    

### 3. Spoofing Source IP (`-S`)

If you suspect a specific internal IP (like a jump host or admin PC) is whitelisted, you can spoof that IP.

- **Command:** `sudo nmap <Target> -e tun0 -S <Whitelisted_IP>`
    
- **Note:** You must specify your interface (`-e`) and you usually won't see the results unless you can sniff the traffic elsewhere (since the responses go to the spoofed IP).
    

### 4. Source Port Manipulation (`--source-port`)

Firewalls often blindly trust traffic coming from specific ports, such as **DNS (53)**, **HTTP (80)**, or **FTP (20)**.

- **The Trick:** Force Nmap to send all its probe packets _from_ port 53.
    
- **Command:** `sudo nmap <Target> --source-port 53`
    

---

## 🛠️ Summary of Evasion Commands

|**Strategy**|**Command Option**|**logic**|
|---|---|---|
|**ACK Scan**|`-sA`|Bypasses "Stateful" firewall checks.|
|**Decoys**|`-D RND:10`|Obfuscates your IP among 10 others.|
|**Spoof Source IP**|`-S <IP>`|Pretends to be a trusted machine.|
|**Fixed Source Port**|`--source-port 53`|Exploits misconfigured "Trusted Port" rules.|
|**No Ping**|`-Pn`|Skips host discovery (Firewalls often block ICMP).|
|**Packet Trace**|`--packet-trace`|**Crucial:** Shows you exactly which flag (SYN/ACK/RST) is triggering the response.|