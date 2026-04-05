**Category:** #Exploitation / #Post-Exploitation
**Scope:** Linux & Windows

---

## 🎯 The Core Objective

> **Logic:** You have gained a "Foothold" (initial access). Now, you must break out of the constraints of a low-privileged service or user account to gain full administrative control.

### 🧭 Direction of Travel

In a penetration test, your movement follows two paths:

Code snippet

```
graph TD
    A[Initial Foothold: www-data] -->|Lateral| B[Standard User: jsmith]
    B -->|Vertical| C[SuperUser: root / SYSTEM]
    
    style A fill:#f96,stroke:#333
    style C fill:#f66,stroke:#333,stroke-width:4px
```

|**Movement Type**|**Description**|**Example**|
|---|---|---|
|**Lateral**|Moving between accounts with _similar_ privilege levels.|Moving from `service_acc` to `developer_user`.|
|**Vertical**|Moving from a lower privilege level to a _higher_ one.|Moving from `standard_user` to `root` (Linux) or `SYSTEM` (Windows).|

---

## 🔑 Key Pillars of PrivEsc

### 1. The Sudoers Configuration (`/etc/sudoers`)

- **Logic:** This file is the "Rulebook" for permissions.
    
- **The Vulnerability:** If a user is allowed to run a specific binary (like `vim`, `find`, or `nmap`) as `sudo` without a password, that binary can often be "broken out of" to spawn a root shell.
    
- **Reference:** Always check `sudo -l` immediately upon gaining a shell.
    

### 2. Sensitive File Hunting (The "Breadcrumbs")

Users are human; they make mistakes. Always investigate these locations:

- **`.ssh/`**: Private keys (`id_rsa`) for passwordless login.
    
- **`.bash_history`**: Commands previously run (often contains passwords or reveals hidden scripts).
    
- **Config Files**: Look for `web.config`, `wp-config.php`, or `.env` files—they are goldmines for database credentials.