**Module:** #Network_Enumeration_with_Nmap
**Level:** #Intermediate
**Protocol:** #TCP #SYN

---

## 🔍 Tool Overview

**Nmap (Network Mapper)** is an open-source tool for network discovery and security auditing. It uses raw IP packets to identify available hosts, services, versions, and operating systems.

### Core Architecture

1. **Host Discovery:** Is the target alive?
    
2. **Port Scanning:** Which ports are open?
    
3. **Service Detection:** What is running on those ports?
    
4. **OS Detection:** What is the underlying platform?
    
5. **NSE (Nmap Scripting Engine):** Scripted interaction for vulnerabilities.
    

---

## ⚡ The Default Stealth Scan: TCP SYN (`-sS`)

The SYN scan is the most popular technique because it is fast (scanning thousands of ports per second) and relatively "stealthy" (it never completes the TCP handshake).

### 🛰️ The "Half-Open" Logic

Nmap identifies port states based on how the target responds to a `SYN` packet:

|**Target Response**|**Nmap Port State**|**Technical Meaning**|
|---|---|---|
|**SYN-ACK**|`Open`|Service is active. Nmap sends an **RST** to close immediately.|
|**RST** (Reset)|`Closed`|Port is reachable, but no service is listening.|
|**No Response**|`Filtered`|Packet was dropped/ignored by a **Firewall** or **IDS**.|

---

## 🛠️ Syntax & Command Library

### Basic Scans
```
# Perform a Stealth SYN Scan (Requires sudo for raw socket access)
sudo nmap -sS <target>

# Scan specific ports
sudo nmap -sS -p 22,80,443 <target>

# Scan top 100 ports (Faster than default top 1000)
sudo nmap -sS --top-ports 100 <target>
```

### Reference: Common Scan Techniques

|**Flag**|**Scan Type**|**Description**|
|---|---|---|
|`-sS`|**SYN Scan**|Default, fast, "Half-Open" stealth scan.|
|`-sT`|**Connect Scan**|Completes handshake. Used when sudo is unavailable.|
|`-sU`|**UDP Scan**|Scans stateless UDP services (DNS, DHCP, SNMP).|
|`-sA`|**ACK Scan**|Used to map out firewall rulesets.|

**Local vs. Remote Discovery Logic:**

- **Local Subnet:** Nmap uses **ARP** by default. ARP is rarely blocked by host firewalls, making it the most reliable discovery method.
    
- **Remote Subnet:** Nmap uses **ICMP** (and other methods) because ARP does not travel across routers.
    
- **Force ICMP:** Use `--disable-arp-ping` to see ICMP responses (and TTL values) on a local network.

---

## 📊 Interpreting Output

When Nmap finishes, it provides a table:

- **PORT:** The port number and protocol (e.g., `80/tcp`).
    
- **STATE:** `open`, `closed`, or `filtered`.
    
- **SERVICE:** The inferred service (e.g., `http`).
    

> [!IMPORTANT]
> 
> Always run Nmap with `sudo` when performing SYN scans (`-sS`). Without root privileges, Nmap defaults to a Connect scan (`-sT`), which is noisier and more easily logged by the target.

