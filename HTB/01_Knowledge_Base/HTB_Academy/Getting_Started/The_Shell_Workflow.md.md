**Category:** #Exploitation / #Methodology
**Phase:** Foothold ➔ Persistence

---

## 🔍 Overview

> **Logic:** Getting a shell isn't a random event; it’s a structured sequence of actions. You move from **Passive Observation** to **Active Interaction** to **Established Access**.

---

## 🛠️ The Phase-by-Phase Breakdown

### 1. Enumeration (The "Scan") 🛰️

- **Action:** Find the service and the "Internal Name" of the target (e.g., a web server named `Tom`).
    
- **Goal:** Identify the technology stack (PHP? ASP.NET? Python?).
    

### 2. Discovery (The "Vulnerability") 🔍

- **Action:** Hunt for "Input Vectors."
    
- **Targets:** * **File Uploads:** Can I upload a script?
    
    - **Search Bars:** Can I inject commands (Command Injection)?
        
    - **Login Forms:** Can I bypass authentication?
        

### 3. Delivery (The "Payload") 📦

- **Web Shell Path:** Upload `shell.php` to the webroot.
    
- **Reverse Shell Path:** Inject a `bash -i` or `powershell.exe` string into an input field to trigger a callback.
    

### 4. Interaction (The "Handshake") 🤝

- **Web Shell:** Navigate to `site.com/uploads/shell.php?cmd=whoami`.
    
- **Reverse Shell:** Check your `nc -lvnp` listener. _Did "Tom" call you back?_
    

---

## 🔄 Workflow Visualization

Code snippet

```
graph TD
    A[1. Enumeration] -->|Find Input Vector| B[2. Discovery]
    B -->|Upload Script| C[3a. Web Shell]
    B -->|Command Injection| D[3b. Reverse Shell]
    C -->|HTTP Request| E[4. Interaction]
    D -->|Netcat Listener| E
    E --> F[5. Upgrade to TTY Shell]
    
    style A fill:#f9f,stroke:#333
    style F fill:#00ff00,stroke:#333,color:#000
```

---

## 📊 Comparison: Web Shell vs. Reverse Shell

|**Feature**|**Web Shell**|**Reverse Shell**|
|---|---|---|
|**Persistence**|High (Survives reboots)|Low (Process dies on exit)|
|**Interactivity**|Low (Non-interactive)|High (Interactive Bash/PS)|
|**Firewall Risk**|Very Low|Moderate (Outbound check)|