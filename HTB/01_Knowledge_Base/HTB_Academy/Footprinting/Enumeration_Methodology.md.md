# Enumeration Methodology (The 6-Layer Model)

**Module:** #Footprinting
**Location:** `01_Knowledge_Base/Methodologies/Enumeration_Methodology.md`

🎯 **Objective**

To follow a standardized, static methodology that ensures thorough coverage of a target environment while allowing for dynamic adaptations.

🔍 **Overview**

Enumeration is visualized as passing through **6 Layers** (or boundaries). We move from the outside (Internet Presence) to the deep inside (OS Setup).

---

### 🛡️ The 6-Layer Breakdown

|**Layer**|**Name**|**Focus**|**Information Categories**|
|---|---|---|---|
|**1**|**Internet Presence**|External footprinting.|Domains, Subdomains, IP ranges (Netblocks), ASN, Cloud instances.|
|**2**|**Gateway**|Security measures/protections.|Firewalls, IDS/IPS, EDR, Proxies, VPNs, WAF (Cloudflare).|
|**3**|**Accessible Services**|Functional interfaces.|**(Main focus of this module)** Service types, versions, and configurations.|
|**4**|**Processes**|Internal data flow.|PIDs, source/destination of data, system tasks.|
|**5**|**Privileges**|Permissions/Access levels.|Users, Groups, Environment restrictions, sudo/admin rights.|
|**6**|**OS Setup**|System architecture.|OS Type, Patch level, Config files, sensitive private files.|

---

### 🛠️ Methodology vs. Cheat Sheet

- **Methodology (The Map):** The systematic process of exploring a target. It stays the same regardless of the tool.
    
- **Cheat Sheet (The Gear):** The specific tools (Nmap, Netcat, etc.) and commands used to execute the methodology. Tools change; the methodology remains static.
    

### 🧠 Strategic Concepts

- **The Labyrinth Analogy:** Penetration testing is a maze of "gaps" (vulnerabilities). Not every gap leads to the center (the goal). Enumeration helps you find the _effective_ path rather than forcing your way through a dead end.
    
- **Infrastructure vs. Host vs. OS:**
    
    1. **Infrastructure-based:** Layers 1 & 2.
        
    2. **Host-based:** Layers 3, 4, & 5.
        
    3. **OS-based:** Layer 6.