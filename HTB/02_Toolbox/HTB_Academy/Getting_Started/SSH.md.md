**Category:** #Access / #Persistence 
**Scope:** Remote Administration

---

## 🔍 Overview

> **Logic:** SSH provides a secure, encrypted channel over an unsecured network. In HTB, finding an SSH password or a private key (`id_rsa`) usually means you have moved from a "Foothold" to "Lateral Movement."

---

## 🛠️ Essential Commands

### 1. Basic Connection

The standard way to log in with a password:
```
ssh [user]@[IP]
```

- **Custom Port:** `ssh [user]@[IP] -p [port]` (Common in HTB to see SSH on 2222 or 22022).
    

### 2. Connecting with a Private Key

If you find an `id_rsa` file in a `.ssh` directory:
```
ssh -i id_rsa [user]@[IP]
```

---

## ⚠️ The "Permissions" Gotcha

SSH is very picky about security. If your private key file is "too open" (anyone can read it), SSH will refuse to use it.

**The Fix:
```
chmod 600 id_rsa
```

_(This tells Linux: "Only the owner can read/write this file.")_

---

## 🚀 Pro-Tips for Hackers

- **Accepting New Hosts:** If you get a warning about "Host authenticity," type `yes` to add the target to your `known_hosts` file.
    
- **Executing a Single Command:** You can run a command without staying in the shell: `ssh user@IP "cat /root/root.txt"`
    
- **SSH Config:** If you find yourself connecting to the same box over and over, you can create a config file in `~/.ssh/config` to save the IP and port.

---

## 🏴‍☠️ Tactical Procedure: Looting & Using SSH Keys

> **Scenario:** You have gained a low-level shell (Web Shell/Reverse Shell) and discovered a private key belonging to a higher-level user or `root`.

### 🛠️ The "Key-Looting" Workflow

1. **Find and Extract the Key:** Look for the hidden `.ssh` directory in the user's home or root.
    
    Bash
    
    ```
    cat /root/.ssh/id_rsa
    # OR
    cat /home/[user]/.ssh/id_rsa
    ```
    
    _Copy the entire block of text, including the `-----BEGIN OPENSSH PRIVATE KEY-----` and `-----END...` lines._
    
2. **Transfer to Local Machine:** On your **Kali/Pwnbox**, create a new file and paste the content:
    
    Bash
    
    ```
    nano id_rsa
    ```
    
3. **🛡️ The Golden Rule (Fix Permissions):** SSH will ignore the key if it is "too world-readable." You **must** restrict it:
    
    Bash
    
    ```
    chmod 600 id_rsa
    ```
    
4. **Execute the Login:**
    
    Bash
    
    ```
    ssh [user]@[IP] -i id_rsa -p [PORT]
    ```