**Category:** #Networking / #Exploitation
**Scope:** Scanning, Shells, and Data Transfer

---

## 🔍 Overview

> **Logic:** Netcat reads and writes data across network connections using TCP or UDP. It is simple, lightweight, and incredibly powerful because it doesn't care _what_ data it's sending—it just opens the pipe.

---

## 🛠️ The "Two-Way" Functionality

### 1. The Listener (Waiting for a Connection)

Use this on your **Attack Machine** to catch a reverse shell.
```
# -l (listen), -v (verbose), -n (no DNS), -p (port)
nc -lvnp 4444
```

### 2. The Connector (Initiating a Connection)

Use this to grab banners or connect to a Bind Shell on a **Target**.
```
# -n (no DNS), -v (verbose)
nc -nv [Target_IP] [Port]
```

---

## 🛰️ Tactical Use Cases

### 🚩 Manual Banner Grabbing

If Nmap is being blocked or giving messy results, do it manually to see the raw "Greeting" from the service.
```
# Example: Grabbing an FTP banner on Port 21
nc -nv 10.129.42.253 21
```

### 📁 File Transfer

If `wget` or `curl` aren't available, Netcat can move files.

- **On Receiver:** `nc -l -p 1234 > out.file`
    
- **On Sender:** `nc [Receiver_IP] 1234 < in.file`
    

### 🔍 Simple Port Scanning

A quick, quiet way to check for open ports without the "noise" of Nmap.
```
# -z (Zero-I/O mode - just scan), -w (timeout)
nc -zv 10.129.42.253 20-80
```

---

## 🚩 Flag Reference Table

|**Flag**|**Meaning**|**Why use it?**|
|---|---|---|
|**`-l`**|Listen|To wait for a target to "Phone Home" (Reverse Shell).|
|**`-v`**|Verbose|To see if the connection actually succeeded.|
|**`-n`**|No DNS|Faster. Prevents your IP from appearing in DNS logs.|
|**`-p`**|Port|Specifies which port to use.|
|**`-u`**|UDP|Switches from TCP to UDP mode (for DNS/SNMP).|