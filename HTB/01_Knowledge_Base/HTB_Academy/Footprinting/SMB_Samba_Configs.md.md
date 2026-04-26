**Module:** #Footprinting
**Location:** `01_Knowledge_Base/Protocols/SMB_Samba_Configs.md`

🎯 **Objective**

Detailed breakdown of Samba settings and the administrative "logic" that leads to security gaps.

### ⚙️ 1. Samba Configuration Breakdown (`/etc/samba/smb.conf`)

The file is structured into `[global]` settings and specific `[sharename]` blocks.

#### A. Global Settings (`[global]`)

These apply to the entire server unless overridden by a share.

- **`workgroup`**: The Domain or Workgroup name (e.g., `DEV.INFREIGHT.HTB`).
    
- **`server string`**: The server's identification banner (e.g., `DEVSMB`).
    
- **`log file`**: Where logs are stored, often using `%m` to create a separate log per client.
    
- **`server role`**: Defines if it's a `standalone server`, `domain controller`, or `member server`.
    
- **`map to guest = bad user`**: Crucial setting. If a login fails, the server treats the user as a "Guest" rather than rejecting them.
    

#### B. Share-Specific Settings

|**Setting**|**Description**|
|---|---|
|**`path`**|The actual directory on the Linux filesystem being shared.|
|**`browseable`**|If `yes`, the share shows up when an attacker runs `smbclient -L`.|
|**`public` / `guest ok`**|If `yes`, no password is required to connect.|
|**`read only`**|If `no`, users can modify/delete files.|
|**`create mask`**|Defines permissions for new files (e.g., `0777` = Full permissions).|
|**`directory mask`**|Defines permissions for new directories.|

### ⚠️ 2. The "Attacker's Dream" Config (Dangerous Settings)

Administrators often leave these active for "testing" or "convenience," providing an easy entry point:

|**Dangerous Setting**|**Why it's a risk**|
|---|---|
|**`writable = yes`**|Allows an attacker to upload files (think: webshells or malware).|
|**`enable privileges = yes`**|Honors privileges assigned to specific SIDs (Security Identifiers).|
|**`logon script`**|Executes a specific `.sh` or `.bat` script when a user logs in (Potential for persistence).|
|**`magic script`**|A script executed by the server when closed—rare but highly dangerous.|
|**`usershare allow guests = yes`**|Allows non-authenticated users to see and access shares.|