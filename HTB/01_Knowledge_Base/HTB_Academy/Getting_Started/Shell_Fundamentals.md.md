**Category:** #Exploitation / #Access
**HTB Module:** [[Getting Started]] / [[Shells & Payloads]]

---

## 🔍 Overview

> **Logic:** Once Remote Code Execution (RCE) is achieved, we need a stable interface to the OS (Bash/PowerShell). Since we rarely have SSH/WinRM credentials initially, we use **Volatile Shells** to gain our foothold.

---

## 🔄 1. Reverse Shell (The "Phone Home")

**Concept:** The **Target** initiates the connection to **You**.

### 🛠️ Workflow

1. **Attacker:** Set up a listener (e.g., `nc -lvnp 4444`).
    
2. **Target:** Execute a payload pointing to the Attacker's IP/Port.
    

- **✅ Pros:** Usually bypasses **Egress** (outbound) firewalls—most servers are allowed to "talk" to the internet.
    
- **❌ Cons:** "Fragile." If the network blips or the process is killed, the shell is lost.
    

---

## 🕸️ 2. Bind Shell (The "Open Door")

**Concept:** The **Target** opens a port and waits for **You** to connect.

### 🛠️ Workflow

1. **Target:** Execute a payload that binds a shell to a local port (e.g., 4444).
    
2. **Attacker:** Connect to the Target’s IP on that port (`nc [Target_IP] 4444`).
    

- **✅ Pros:** Persistent as long as the process runs. You can disconnect and reconnect easily.
    
- **❌ Cons:** Almost always blocked by **Inbound** firewalls.
    

---

## 🌐 3. Web Shell (The "Script Hook")

**Concept:** A script file (PHP, ASPX, JSP) uploaded to the webroot.

**Communication:** Commands are sent as HTTP parameters (e.g., `shell.php?cmd=id`).

- **✅ Pros:** Stealthy. It uses standard web traffic (Port 80/443). Survives reboots.
    
- **❌ Cons:** **Non-interactive.** You cannot use commands like `su` or `sudo` easily because the session doesn't "stay open" between requests.
    

---

## 📊 Tactical Comparison

| **Feature**           | **Reverse Shell**      | **Bind Shell**        | **Web Shell**      |
| --------------------- | ---------------------- | --------------------- | ------------------ |
| **Traffic Direction** | Outbound (Target ➔ Us) | Inbound (Us ➔ Target) | HTTP Request       |
| **Firewall Bypass**   | High (Egress)          | Low (Inbound blocked) | High (Web Traffic) |
| **Interactivity**     | High                   | High                  | Low                |
| **Persistence**       | Low (Process based)    | Medium                | High (File based)  |