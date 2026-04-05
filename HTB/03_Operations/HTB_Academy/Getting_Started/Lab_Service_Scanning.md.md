**Date:** 2026-03-22
**Category:** #Enumeration / #Nmap / #SMB
**Target IP:** `10.129.24.28` 
**Status:** **Completed ✅**

---

## 🛰️ Phase 1: Service Identification (Port 8080)

> **Objective:** Determine the version of the service running on a known high-number port.

### 🛠️ Execution
```
nmap -sV -sC -p8080 10.129.24.28
```

### 📡 Intelligence

|**Port**|**State**|**Service**|**Version**|
|---|---|---|---|
|**8080/tcp**|Open|http|**Apache Tomcat**|

---

## 🕵️ Phase 2: Finding the Hidden Telnet

> **Objective:** Identify non-standard ports where common services might be hiding.

### ⚡ Step 1: High-Speed Discovery

Standard scans were too slow. Switched to high-rate "Sprayer" mode:
```
nmap -p- --min-rate 5000 10.129.24.28
```

- **Initial Finding:** `2323/tcp open 3d-nfsd` (Suspicious service name).
    

### 🔍 Step 2: The "Surgeon" Scan

Verify the true nature of port **2323**:
```
nmap -sV -sC -p2323 10.129.24.28
```

- **True Service:** `telnet` (Linux telnetd).
    

---

## 📂 Phase 3: SMB Share Exploitation

> **Objective:** Access restricted file shares using discovered credentials.

### 🛠️ Execution Flow

1. **Authentication:** Connected to the `users` share as user `bob`.
    ```
    smbclient -U bob //10.129.24.28/users
    ```
    
2. **Navigation:** Navigated to the `flag` directory and listed contents.
    
3. **Extraction:** Downloaded `flag.txt` to the local attack machine.
    ```
    smb: \flag> get flag.txt
    ```
    

### 🚩 Flag Recovery
```
cat flag.txt
# Result: dceece590f3284c3866305eb2473d099
```

---

## 🧠 Lessons Learned

- **Service Misdirection:** Port 2323 was a decoy (labeled `3d-nfsd`). **Version scanning (`-sV`) is non-negotiable** for uncovering the truth behind a port.
    
- **Speed vs. Accuracy:** Using `--min-rate 5000` allowed for a full port discovery in seconds, preventing time-waste on firewalled ports.
    
- **SMB Workflow:** `smbclient` is the standard tool for share interaction. **Remember:** You must `get` the file to your local machine before you can read it with `cat`.