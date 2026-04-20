Module: #Network_Enumeration_with_Nmap
Location: `02_Toolbox/HTB_Academy/02_Network_Enumeration_with_Nmap/Section_12_Hard_Lab.md`

🎯 **Objective**

Identify the version of a hidden service on port `50000` after an administrator has "hardened" the environment following an IDS/IPS training course.

🔍 **The "Hardened" Scenario**

- **The Bait:** Port `50000` appears as `tcpwrapped` when scanned normally.
    
- **The Wall:** The IDS/IPS detects and resets (RST) any connection that matches Nmap's service engine fingerprints.
    
- **The Gap:** The firewall is configured to trust traffic from **Source Port 53** (DNS), assuming it is legitimate management communication.
    

🛠️ **Step-by-Step Solution**

### 1. Identify the Target Port

Initial scanning confirms port `50000` is open but protected.
```
sudo nmap <TARGET_IP> -p 50000 -Pn
```

### 2. Solve the Local "Address in Use" Conflict

To bypass the firewall using Source Port 53, you must use a tool like `nc` or `ncat`. However, Pwnbox runs `dnsmasq`, which occupies port 53 locally.

**Commands to clear the port:**
```
# Locate the process ID (PID) using port 53
sudo lsof -i :53

# Kill the process (usually dnsmasq)
sudo kill -9 <PID>
```

### 3. Execute the Stealth Connection

Use Netcat (`nc`) to spoof the source port. This avoids the Nmap "version engine" signature that triggers the IPS.
```
# Connect using source port 53
sudo nc -nv -p 53 <TARGET_IP> 50000
```

### 4. Trigger the Banner

The service is passive and will not send the flag until it receives data.

- **Action:** Once the connection is "Open," type `GET /` and press **Enter**.
    

📋 **Commands Used in this Section**

|**Command**|**Purpose**|
|---|---|
|`sudo lsof -i :53`|Checks for local services blocking your ability to spoof DNS ports.|
|`sudo kill -9 <PID>`|Terminates the local resolver to free up Port 53.|
|`nc -nv -p 53`|Connects to the target while masquerading as DNS traffic.|
|`ncat -u --source-port 53`|(Alternative) Used to test if the service responds to UDP probes.|

✅ **Flag Found**

`HTB{....}`