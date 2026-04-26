**Module:** #Footprinting 
**Location:** `02_Toolbox/Protocols/SMB_RPC_Tools.md`

### 🔍 1. Service Scanning
```
# Nmap SMB scan with default scripts and version detection
sudo nmap <TARGET_IP> -sV -sC -p139,445
```

### 📁 2. SMBClient (Manual Interaction)

- **List Shares (Null Session):** `smbclient -N -L //<TARGET_IP>`
    
- **Connect to Share:** `smbclient //<TARGET_IP>/<SHARENAME>`
    
- **Common Commands (Inside Shell):**
    
    - `ls`: List directory.
        
    - `get <file>`: Download file.
        
    - `!<cmd>`: Run local system command (e.g., `!cat file.txt`).
        
- **Administrative View:** `sudo smbstatus` (View versions and active connections).
    

### ⚡ 3. RPCclient (MS-RPC Functions)

Connect anonymously: `rpcclient -U "" <TARGET_IP>`

|**Command**|**Description**|
|---|---|
|`srvinfo`|Server OS and version info.|
|`enumdomains`|Enumerate all deployed domains.|
|`querydominfo`|Domain, server, and user counts.|
|`netshareenumall`|List all available shares.|
|`netsharegetinfo <share>`|Permissions and path for a specific share.|
|`enumdomusers`|Enumerate all domain users.|
|`queryuser <RID>`|Detailed info for a specific user (Home dir, Logon script, etc).|
|`querygroup <RID>`|Group membership and attributes.|

### 🔨 4. Brute Forcing & Automation

#### Manual RID Brute Forcing (Bash)

Used to find usernames by cycling through Relative Identifiers (RIDs):
```
for i in $(seq 500 1100); do 
  rpcclient -N -U "" <TARGET_IP> -c "queryuser 0x$(printf '%x\n' $i)" | grep "User Name\|user_rid\|group_rid" && echo ""; 
done
```

#### Specialized Tools

- **Samrdump:** `samrdump.py <TARGET_IP>` (Impacket tool for user/domain info).
    
- **SMBMap:** `smbmap -H <TARGET_IP>` (Quickly check permissions).
    
- **CrackMapExec:** `crackmapexec smb <TARGET_IP> --shares -u '' -p ''` (Broad enumeration).
    
- **Enum4Linux-ng:**
    ```
    # Installation
    git clone https://github.com/cddmp/enum4linux-ng.git && cd enum4linux-ng && pip3 install -r requirements.txt
    # Full Enumeration (-A)
    ./enum4linux-ng.py <TARGET_IP> -A
    ```