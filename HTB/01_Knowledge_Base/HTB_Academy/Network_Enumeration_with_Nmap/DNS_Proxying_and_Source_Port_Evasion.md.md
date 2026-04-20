# Concept: DNS Proxying & Trusted Port Evasion

**Category:** #Networking_Concepts / #Evasion_Techniques
**File Name:** `DNS_Proxying_and_Source_Port_Evasion.md`
**Location:** `01_Knowledge_Base/Defensive_Systems/`

---

## 🏗️ The DNS Vulnerability

DNS (Domain Name System) primarily uses **UDP 53**, but it also uses **TCP 53** for zone transfers and large queries. Because DNS is "critical infrastructure," firewalls are often configured with a "Trusted Port" rule:

- _Rule:_ "If the traffic is coming **from** Port 53, let it through, because it's probably just a DNS server responding to a query."
    

### 🔓 The Exploit: Source Port Manipulation

Attackers can force their tools (Nmap, Netcat) to use **Port 53 as the Source Port**. To the firewall, your malicious scan looks like a legitimate DNS response.

|**Command**|**Purpose**|
|---|---|
|`--source-port 53`|Forces Nmap to send all probe packets from port 53.|
|`ncat --source-port 53`|Establishes a full connection through the firewall hole.|

---

## 🗺️ DNS Proxying in the DMZ

A **DMZ (Demilitarized Zone)** is a subnetwork that contains a company's external-facing services (like web servers). These servers often have a special relationship with **Internal DNS Servers**.

### Why use `--dns-server`?

If you are inside a network (or a DMZ) and try to scan, the external DNS (like Google 8.8.8.8) might not know the names of internal computers (e.g., `payroll-db.internal`).

- By pointing Nmap to the **Company's Internal DNS Server** using `--dns-server <Internal_IP>`, you can "ask" the internal server to reveal the locations of private hosts that are hidden from the outside world.
    

---

## 🔬 Comparison: Regular vs. Trusted Port Scan

### ❌ Regular Scan (Blocked)
```
sudo nmap 10.129.2.28 -p50000 -sS -Pn -n
```

- **Traffic:** Source Port: 33436 (Random) ➡️ Destination Port: 50000.
    
- **Firewall Result:** "I don't know Port 33436. **BLOCK.**"
    

### ✅ DNS-Source Scan (Bypassed)
```
sudo nmap 10.129.2.28 -p50000 -sS -Pn -n --source-port 53
```

- **Traffic:** Source Port: **53 (DNS)** ➡️ Destination Port: 50000.
    
- **Firewall Result:** "Oh, it's from Port 53! That's just DNS. **PASS.**"