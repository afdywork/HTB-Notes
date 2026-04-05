## 🔍 Overview

When a system is compromised via Remote Code Execution (RCE), we need a persistent way to communicate with the OS (Bash/PowerShell). While SSH or WinRM are ideal, they require credentials we usually don't have yet. Thus, we use **Shells**.

## 🔄 1. Reverse Shell (Most Common)

- **Concept:** The **Target connects to us**.
    
- **Workflow:** 1. Start a `netcat` listener on your attack machine. 2. Execute a payload on the target that points back to your IP/Port.
    
- **Pros:** Usually bypasses egress (outbound) firewalls.
    
- **Cons:** "Fragile"—if the process stops or the connection blips, the shell dies.
    

## 🕸️ 2. Bind Shell

- **Concept:** The **Target opens a port** and waits for us to connect.
    
- **Workflow:**
    
    1. Execute a payload on the target that binds a shell to a local port.
        
    2. Connect to that target's IP/Port using `nc`.
        
- **Pros:** If you lose the connection, you can simply reconnect (as long as the process is still running).
    
- **Cons:** Inbound firewalls almost always block unknown ports.
    

## 🌐 3. Web Shell

- **Concept:** A script (PHP, ASPX, JSP) uploaded to the webroot.
    
- **Communication:** Conducted via HTTP parameters (e.g., `?cmd=id`).
    
- **Pros:** Bypasses firewalls (uses Port 80/443); survives reboots.
    
- **Cons:** Non-interactive (you have to refresh/send new requests for every command).