# Operation Notes: SMB & RPC Enumeration

**Target IP:** `10.129.17.207` **Protocol:** SMB (TCP 139/445)

## 📋 Executive Summary

The engagement focused on the enumeration of a Samba-based file server. Through a combination of service scanning, RPC querying, and manual share interaction, we successfully identified server versions, domain memberships, hidden metadata, and sensitive file contents.

---

## 🔍 Methodology & Command History

### 1. Service Identification (Question 1)

**Objective:** Identify the exact version and "banner" of the running service.

- **Initial Scan:** Used `nmap` to find the software version.
    ```
    sudo nmap -sV -sC 10.129.17.207 -p445
    ```
    
- **Result:** Identified `Samba smbd 4.6.2`.
    
### 2. Share & Domain Enumeration (Questions 2 & 4)

**Objective:** Discover accessible network shares and domain membership.

- **Command:**
    ```
    rpcclient $> netshareenumall   # List all shares
    rpcclient $> enumdomains       # Identify domain name
    ```
    
- **Findings:** * **Accessible Share:** `sambashare`
    
    - **Domain:** `DEVSMB`
        
### 3. File System Interaction (Question 3)

**Objective:** Access the files inside the discovered share to retrieve the flag.

- **Tool:** `smbclient` (Standard tool for file browsing).
    ```
    # Connect to the share
    smbclient -N //10.129.17.207/sambashare
    
    # Internal Navigation (Do NOT use '!' for remote files)
    smb: \> ls
    smb: \> cd contents
    smb: \> ls
    
    # Retrieval
    smb: \contents\> get flag.txt
    smb: \contents\> !cat flag.txt   # Read locally or use 'more flag.txt'
    ```
    
### 4. Metadata & Path Analysis (Questions 5 & 6)

**Objective:** Find hidden "customized" information and the server's internal directory structure.

- **Tool:** `rpcclient` (Detailed metadata querying).
    
- **Command:**
    ```
    rpcclient $> netsharegetinfo sambashare
    ```
    
- **Findings:**
    
    - **Customized Version:** Found in the `remark:` field (e.g., `v3.1`).
        
    - **Full System Path:** Found in the `path:` field (e.g., `/home/sambauser/`).