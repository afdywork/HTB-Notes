**Category:** #Reconnaissance / #Enumeration 
**Service Port:** `161` (UDP)

---

## 🔍 Overview

> **Logic:** SNMP is used to monitor network-attached devices. Information is organized in a tree structure called a **MIB** (Management Information Base), and items are identified by an **OID** (Object Identifier).

### 🔑 The "Community String"

This is the plain-text password required to query the device.

- **`public`**: Standard read-only string (Commonly left as default).
    
- **`private`**: Standard read-write string.
    

---

## 🛠️ Essential Commands

### 1. Manual Enumeration (`snmpwalk`)

If you know the community string, you can "walk" the tree to dump information.

Bash

```
# Syntax: snmpwalk -v [version] -c [community] [IP] [OID]
snmpwalk -v 2c -c public 10.129.42.253 1.3.6.1.2.1.1.5.0
```

- **`-v 2c`**: Specifies the version (2c is most common in labs).
    
- **`-c public`**: The community string/password.
    

### 2. Brute Forcing Community Strings (`onesixtyone`)

If `public` doesn't work, you need to guess the string using a wordlist.

Bash

```
onesixtyone -c /usr/share/wordlists/metasploit/snmp_default_pass.txt 10.129.42.254
```

---

## 🕵️ What to Look For (High-Value OIDs)

When you walk an SNMP device, look for these "Gold Mines":

- **System Description:** OS version and kernel info.
    
- **Running Processes:** Can reveal security software or vulnerable apps.
    
- **Network Interfaces:** Reveals internal IP addresses/subnets (useful for pivoting).