# FTP & TFTP: Protocol Mechanics & Configs

**Module:** #Footprinting
**Location:** `01_Knowledge_Base/Protocols/FTP_TFTP_Deep_Dive.md`

🎯 **Objective**

Understand the underlying mechanics of file transfer protocols and identify the dangerous configuration patterns that lead to exploitation.

---

### 📡 1. FTP Communication Channels

FTP is unique because it uses **two separate channels** for communication.

- **Control Channel (Port 21):** Used for sending commands (USER, PASS, LIST, etc.) and receiving status codes.
    
- **Data Channel (Port 20):** Used exclusively for the actual transfer of data.
    

#### Active vs. Passive Mode

- **Active FTP:** * Client connects to Port 21 and tells the server which port to use for data.
    
    - **Problem:** If the client is behind a firewall, the server's attempt to connect _back_ to the client is blocked.
        
- **Passive FTP:**
    
    - The server tells the client which port to connect to for the data channel.
        
    - **Benefit:** Since the client initiates both connections, it bypasses most client-side firewalls.
        

---

### ⚙️ 2. vsFTPd Configuration (`/etc/vsftpd.conf`)

This is the "Source of Truth" for the most common Linux FTP server. A "Safe" configuration often hides vulnerabilities in the comments.

#### Standard/Baseline Settings

|**Setting**|**Default/Recommended**|**Description**|
|---|---|---|
|`listen=NO`|NO|If NO, runs from `inetd` instead of standalone.|
|`local_enable=YES`|YES|Allows local system users to log in.|
|`xferlog_enable=YES`|YES|Logs all uploads and downloads.|
|`connect_from_port_20=YES`|YES|Ensures data transfers originate from port 20.|
|`secure_chroot_dir`|`/var/run/vsftpd/empty`|Empty directory used for security isolation.|

#### ⚠️ Dangerous/High-Risk Settings

These are the settings we hunt for during enumeration:

- **`anonymous_enable=YES`**: Enables the `anonymous` account.
    
- **`no_anon_password=YES`**: Server won't even ask for a password from anonymous users.
    
- **`anon_upload_enable=YES`**: Anonymous users can upload (Risk: Malware/Webshells).
    
- **`anon_mkdir_write_enable=YES`**: Anonymous users can create directories.
    
- **`anon_root=/path/to/dir`**: Changes the home directory for anonymous users. If this points to a web root, it's an immediate RCE path.
    
- **`ls_recurse_enable=YES`**: Allows `ls -R`. Massive info leak.
    
- **`hide_ids=YES`**: Masks the UID/GID of files as "ftp", preventing us from seeing which local user owns a sensitive file.
    

---

### 🛡️ 3. Security Mechanisms

- **`/etc/ftpusers`**: A critical file containing a **Blacklist**. Any username listed here (e.g., `root`, `bin`, `guest`) is forbidden from logging in via FTP, even with a valid password.
    
- **Banner (Response Code 220)**: Often leaks the specific software version and OS type.
    
- **TLS/SSL**: If `ssl_enable=YES`, traffic is encrypted. We use `openssl` to extract hostnames and email addresses from the certificate.
    

---

### ☁️ 4. TFTP (Trivial FTP) Mechanics

TFTP is the "stripped-down" cousin of FTP. It is **unreliable** and **unsecured**.

- **No Authentication:** There is no `USER` or `PASS`.
    
- **UDP Port 69:** Unlike FTP (TCP), TFTP uses UDP. If a packet is lost, the application layer must handle the recovery.
    
- **Visibility:** TFTP cannot list directories (`ls` does not exist). You must know the exact filename to download it.
    
- **Environment:** Usually only found in local, protected networks for booting diskless workstations or transferring config files to routers/switches.