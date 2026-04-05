🏷️ Tags: #HTB #Enumeration #SMB #Nmap #Tomcat #Telnet
**Target IP:** `10.129.24.28` 
**Date:** 2026-03-22 

#### **Question 1**:
Perform an Nmap scan of the target. What does Nmap display as the version of the service running on port 8080?

**Step 1: Run nmap with specific port and target IP Address**:
nmap -sV -sC -p8080 10.129.24.28

**Result**:
PORT     STATE SERVICE VERSION
8080/tcp open  http    **Apache Tomcat**

**Findings**:
Port: 8080/TCP
Service: http
**Version: Apache Tomcat** (Answer)

#### **Question 2**:
Perform an Nmap scan of the target and identify the non-default port that the telnet service is running on.

**Step 1: Run nmap command to check all ports**:
nmap -sV -sC -p- 10.129.24.28 (took long time)

**Step 2: Run nmap command to check all ports using min rate **:
nmap -p- --min-rate 5000 10.129.24.28 (got result)

**First Result**:
**2323/tcp open  3d-nfsd (Suspicious port 2323 with service name 2d-nfsd)**

**Step 3: Run nmap command to the suspicious port**
nmap -sV -sC -p2323 10.129.24.28

**Final Result**:
PORT     STATE SERVICE VERSION
2323/tcp open  **telnet**  Linux telnetd

**Findings**:
Port: 2323/TCP
**Service: telnet** (Answer)

#### **Question 3:
List the SMB shares available on the target host. Connect to the available share as the bob user. Once connected, access the folder called 'flag' and submit the contents of the flag.txt file.

**Step 1: Connect to the 'users' share as Bob**
smbclient -U bob \\\\10.129.24.28\\users
Password used: Welcome1 (from previous notes)

**Step 2: Navigation & Download**
smb: \> cd flag
smb: \flag\> ls

**Found flag.txt**
smb: \flag\> get flag.txt
smb: \flag\> exit

**Step 3: Reading the Loot**
cat flag.txt

**Flag Result**: `dceece590f3284c3866305eb2473d099` (Answer)

===========================================
## Lessons Learned

1. **Service Misdirection:** Port `2323` was a decoy. Version scanning (`-sV`) is non-negotiable for finding the truth.
    
2. **Speed vs. Accuracy:** Used `--min-rate 5000` to find ports in seconds instead of minutes.
    
3. **SMB Workflow:** `smbclient` is essential for checking network shares. Remember: `get` downloads the file to your local machine; you cannot `cat` it inside the SMB prompt.