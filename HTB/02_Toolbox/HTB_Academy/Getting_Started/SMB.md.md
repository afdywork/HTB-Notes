**Category:** #Enumeration / #Exploitation
**Service Port:** `445` (TCP) / `139` (NetBIOS)

---

## 🔍 Overview

> **Logic:** SMB is used for sharing files, printers, and serial ports. In a Windows environment, it is the backbone of the network. For an attacker, it is a primary target for **Information Gathering** and **Lateral Movement**.

---

## 🛠️ Essential Commands (`smbclient`)

### 1. The "Null Session" (Anonymous Check)

Always check if the server allows you to look at the shares without a password.
```
# -N: No password | -L: List shares
smbclient -N -L //10.129.42.253
```

### 2. Authenticated Connection

If you have a username (e.g., `bob`) and a password:
```
# Syntax: smbclient -U [user] //[IP]/[Share]
smbclient -U bob //10.129.42.253/users
```

---

## 🎮 Inside the SMB Prompt

Once you are connected (you'll see a `smb: \>` prompt), use these standard commands to move around:

|**Command**|**Action**|
|---|---|
|**`ls`**|List files and folders in the current share.|
|**`cd [dir]`**|Enter a specific directory.|
|**`get [file]`**|**Download** a file to your local Kali/Pwnbox.|
|**`put [file]`**|**Upload** a file from your machine to the target.|
|**`exit`**|Close the connection.|

---

## 🚀 Pro-Tip: The "Mount" Alternative

If you have a lot of files to look through, instead of using `smbclient`, you can **mount** the share to a folder on your Kali machine so you can use tools like `grep` or `cat` directly:
```
sudo mount -t cifs //10.129.42.253/sharename /mnt/htb -o user=bob,password=p@ssword
```