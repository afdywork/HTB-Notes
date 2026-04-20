**Module:** #Network_Enumeration_with_Nmap
**Path:** `02_Toolbox/HTB_Academy/02_Network_Enumeration_with_Nmap/Service_Enumeration.md`

---

## 🛠️ The Core Concept

Finding an open port is only 50% of the job. To find an exploit, you need the **exact version number** of the application running on that port.

|**Flag**|**Function**|**Benefit**|
|---|---|---|
|**`-sV`**|Service Version Detection|Probes open ports to determine service name and version.|
|**`-v` / `-vv`**|Increase Verbosity|Shows open ports **immediately** as they are found (no waiting for the end).|
|**`--stats-every=5s`**|Status Updates|Automatically prints the progress (ETC) every 5 seconds.|

---

## 🏎️ Efficiency Strategy: The "Quick Peek"

Don't wait for a full scan to start working.

1. Run a quick port scan first.
    
2. Start investigating those ports immediately.
    
3. Run the full scan (`-p-`) in a separate terminal tab in the background.
    

Bash

```
# Recommended "Live View" Full Scan
sudo nmap 10.129.2.28 -p- -sV -v --stats-every=10s
```

---

## 🧬 Banner Grabbing: Manual Verification

Nmap's `-sV` is good, but it can be tricked or suppressed. Sometimes, "grabbing the banner" manually with **Netcat (`nc`)** reveals details Nmap missed (like specific Linux distros: `Ubuntu`).

### 🛠️ Manual Interrogation

If Nmap gives you a vague version, try connecting directly:
```
# Connect to a port to see the banner
nc -nv 10.129.2.28 25
```

### 📡 Network-Level Traffic Analysis

To see exactly how the server "talks" during the banner exchange, use `tcpdump` in a second terminal:
```
# Capture traffic between you and the target
sudo tcpdump -i eth0 host <YOUR_IP> and <TARGET_IP>
```

- **Look for the `[P.]` Flag:** This is the **PSH (Push)** flag. It indicates the server is "pushing" data (the banner) to you immediately after the 3-way handshake.
    

---

## 🔍 Decoding the Nmap Version Engine

1. **Banner Check:** Nmap connects and waits for the service to introduce itself.
    
2. **Signature Matching:** If there's no banner, Nmap sends specific probes and compares the response to a database of thousands of signatures.
    
3. **Packet Trace:** Use `--packet-trace` to see this interrogation in real-time.