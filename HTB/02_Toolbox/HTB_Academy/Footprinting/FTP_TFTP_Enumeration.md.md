# FTP & TFTP Enumeration

**Module:** #Footprinting
**Location:** `02_Toolbox/HTB_Academy/04_Footprinting/FTP_TFTP_Enumeration.md`

🎯 **Objective**

Identify FTP/TFTP service versions, check for anonymous access, and exploit misconfigurations to download sensitive files or upload malicious content.

---

### 🛡️ Protocol Fundamentals

|**Feature**|**FTP (File Transfer Protocol)**|**TFTP (Trivial FTP)**|
|---|---|---|
|**Port**|21 (Control), 20 (Data)|69|
|**Transport**|TCP (Reliable)|UDP (Unreliable)|
|**Auth**|User/Pass (Cleartext)|**No Authentication**|
|**Listing**|Supports `ls`, `dir`|**No directory listing** (Must know filename)|

---

### 🛠️ Interaction & Enumeration Commands

#### 1. Standard FTP Interaction
```
# Connect to target
ftp <TARGET_IP>

# Common Commands once logged in (e.g., as 'anonymous')
ls -R               # Recursive listing (if enabled)
status              # Show current server settings
debug               # Enable debugging output
trace               # Enable packet tracing
get <file>          # Download a specific file
put <file>          # Upload a local file
exit                # Close session
```

#### 2. Mass Downloading (Mirroring)

Useful for local inspection of the entire accessible structure.
```
wget -m --no-passive ftp://anonymous:anonymous@<TARGET_IP>
```

#### 3. Encrypted FTP (FTPS)

If port 21 uses SSL/TLS, standard clients may fail. Use OpenSSL to inspect the certificate.
```
openssl s_client -connect <TARGET_IP>:21 -starttls ftp
```

#### 4. TFTP Interaction

Since there is no `ls` command, you must guess or find filenames via other means (e.g., brute force or documentation).
```
tftp <TARGET_IP>
tftp> get backup.cfg
tftp> status
```

---

### 🔍 Nmap Scripting Engine (NSE)

Automate the discovery of versions and common vulnerabilities.

**Update NSE Database:** `sudo nmap --script-updatedb`

**Targeted FTP Scan:**
```
sudo nmap -sV -p21 -sC -A <TARGET_IP>
```

- `-sC`: Runs default scripts (like `ftp-anon`).
    
- `-sV`: Probes for service/version info.
    
- `--script-trace`: Use this to see the raw FTP commands Nmap sends.
    

**Finding specific FTP scripts:
```
find /usr/share/nmap/scripts/ -name "ftp*"
```

---

### 📋 Critical Configurations (vsFTPd)

Check `/etc/vsftpd.conf` for these high-risk settings during an internal assessment:

|**Setting**|**Risk**|
|---|---|
|`anonymous_enable=YES`|Allows login without a unique account.|
|`anon_upload_enable=YES`|Allows anonymous users to upload files.|
|`anon_mkdir_write_enable=YES`|Allows anonymous users to create directories.|
|`write_enable=YES`|Enables write commands (upload/delete).|
|`ls_recurse_enable=YES`|Allows an attacker to map the entire drive quickly.|
|`/etc/ftpusers`|A **blacklist** of users NOT allowed to use FTP (check for missing names).|