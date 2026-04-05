**Category:** #Reconnaissance / #Fingerprinting
**Phase:** Enumeration

---

## 🔍 Overview

> **Logic:** Banner Grabbing is a method used to query a service to disclose its **Name** and **Version**. It relies on the "Politeness" of network protocols that send a greeting string upon connection.

### 💡 Why it Works

Many common servers (Apache, OpenSSH, vsftpd) are configured by default to send a "Header" or "Banner" that identifies the software.

- **Example:** `SSH-2.0-OpenSSH_8.2p1 Ubuntu-4ubuntu0.5`
    
- **The Intel:** This tells the attacker the exact software (`OpenSSH`), the version (`8.2p1`), and the underlying OS (`Ubuntu`).
    

---

## 🛠️ Methods of Execution

### 1. The Nmap Way (Automated)

Nmap has a built-in script specifically for this task.

Bash

```
nmap -sV --script=banner <target_IP>
```

- `-sV`: Probes open ports to determine service/version info.
    
- `--script=banner`: Specifically attempts to grab the initial text string sent by the service.
    

### 2. The Netcat Way (Manual)

Sometimes automated tools are blocked or give messy output. A manual connection is the "Purist" way.

Bash

```
nc -nv <target_IP> <port>
```

_Once connected, press **Enter** or type `HEAD / HTTP/1.0` for web services to force the banner to appear._

---

## 🛡️ The Defensive Perspective (Hardening)

In a hardened environment, a skilled System Administrator will attempt to hide this information to prevent **Information Leakage**.

- **Banner Masking:** Changing the banner string to something generic (e.g., changing `Apache 2.4.41` to just `Web Server`).
    
- **Disabling Banners:** Configuring the service to send no identification string at all.
    

|**Environment**|**Observation**|**Attacker Effort**|
|---|---|---|
|**Default/Lab**|Clear version strings.|Low (Use Searchsploit immediately).|
|**Hardened/Prod**|Generic or Hidden banners.|High (Requires "TCP Fingerprinting" or behavior analysis).|