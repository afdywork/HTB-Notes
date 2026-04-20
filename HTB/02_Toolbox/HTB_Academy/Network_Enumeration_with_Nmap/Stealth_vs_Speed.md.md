**Module:** #Network_Enumeration_with_Nmap
**Path:** `02_Toolbox/HTB_Academy/02_Network_Enumeration_with_Nmap/Stealth_vs_Speed.md`

---

## 🏎️ The Lab Approach: "Brute Force Speed"

In a controlled environment (CTFs, Labs, Exam), time is your primary constraint. We prioritize getting data quickly over being detected.

### 🛠️ The "Loud" Command
```
nmap -p- --min-rate 5000 10.129.x.x
```

- **Why it’s loud:** * `--min-rate 5000` sends packets faster than any normal human or application would.
    
    - High speed triggers **IDS/IPS (Intrusion Detection/Prevention Systems)** immediately.
        
    - If run without `sudo`, it uses `-sT` (Connect Scan), which fills application logs with thousands of "Connection Established" entries.
        

---

## 🛡️ The Real-World Approach: "Surgical Stealth"

In a professional engagement, the goal is to remain **undetected**. If you are blocked by a firewall or logged by a SOC, your operation is compromised.

### 🛠️ The "Stealth" Command
```
sudo nmap -sS -p- -T3 -Pn -n --randomize-hosts 10.129.x.x
```

### 🔍 Key Stealth Components:

1. **Root Privileges (`sudo -sS`):** Always use the **SYN Scan**. It is a "half-open" scan that doesn't complete the handshake, keeping your IP out of many application-level logs.
    
2. **Timing Templates (`-T3`):** Stick to "Normal" (T3) or "Polite" (T2). This mimics natural network traffic patterns.
    
3. **No Rate Limiting:** Avoid `--min-rate`. Let Nmap's congestion control algorithms handle the speed to avoid "choking" the target's bandwidth.
    
4. **Fragmentation (`-f`):** (Optional) Splits the IP header into smaller pieces to bypass simple packet filters.
    
5. **Decoys (`-D`):** (Advanced) Sends scans from "spoofed" IP addresses alongside your own to hide your true identity in a sea of fake traffic.
    

---

## 📊 Summary Table: Lab vs. Field

|**Feature**|**Lab / Exam Strategy**|**Real World (OPSEC) Strategy**|
|---|---|---|
|**Priority**|**Speed**|**Persistence / Stealth**|
|**Scan Type**|`-sT` (if no sudo) or `-sS`|**Always** `-sS` (SYN Scan)|
|**Rate**|`--min-rate 5000+`|Standard Nmap congestion control|
|**Timing**|`-T4` or `-T5`|`-T3` or `-T2`|
|**Logging Risk**|High (Handshake logs)|Low (Kernel-level only)|
|**Detection Risk**|Guaranteed (Alerts IDS)|Variable (Aims to stay under threshold)|