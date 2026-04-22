# FTP Enumeration: Target 10.129.202.5

**Module:** #Footprinting **Location:** `03_Operations/HTB_Labs/Operations/Lab_FTP.md`

🎯 **Objective** Identify the FTP service version and retrieve the `flag.txt` from the target system.

---

### 🕵️ 1. Service Identification

Initial scan to identify the service and version running on Port 21.

**Command:**
```
sudo nmap -sC -sV -A -p21 10.129.202.5
```

**Intelligence Gained:**

- **Port:** 21/tcp (Open)
    
- **Banner:** `220 InFreight FTP v1.1`
    
- **OS Guess:** Linux 4.15 - 5.8
    
- **Note:** The service returned a custom banner "InFreight FTP v1.1" and a non-standard error message: `"Invalid command: try being more creative"`.
    

---

### 🛠️ 2. Connection & Exploitation Attempts

#### Attempt A: Manual FTP Client

Attempted to connect via standard `ftp` client.

- **Result:** Connection successful, but the client struggled with commands before full authentication.
    
- **Status:** Failed to list or download files manually due to session/bind errors.
    

#### Attempt B: Nmap Scripting (NSE)

Checked for anonymous login support via Nmap scripts.

- **Command:** `sudo nmap -p21 --script ftp-anon 10.129.202.5`
    
- **Result:** Confirmed the port is open, though the script output was slightly obscured by timeouts in the trace.
    

#### Attempt C: Recursive Mirroring (The Solution)

Used `wget` to bypass interactive shell issues and automatically crawl the FTP root using anonymous credentials.

**Command:
```
wget -m --no-passive ftp://anonymous:anonymous@10.129.202.5
```

**Execution Log:**

1. Logged in as `anonymous`.
    
2. Successfully retrieved `.listing`.
    
3. Identified and downloaded:
    
    - `.bash_logout`
        
    - `.bashrc`
        
    - `.profile`
        
    - **`flag.txt`**
        

---

### 🚩 3. Findings

Navigated to the auto-generated directory to inspect the loot.

**Flag Location:** `./10.129.202.5/flag.txt` **Flag Content:** 

---

### 📋 Questions Answered

1. **FTP Version:** `InFreight FTP v1.1`
    
2. **Flag Content:** 