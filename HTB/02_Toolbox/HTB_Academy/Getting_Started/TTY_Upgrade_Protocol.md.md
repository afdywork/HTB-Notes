**Category:** #Exploitation / #Post-Exploitation
**Scope:** Linux / Unix Shells

---

## 🔍 The Problem

> **Logic:** Standard Netcat shells are "Dumb Shells." They don't support **Tab Autocomplete**, **Arrow Keys (History)**, or **Signal Handling**. If you hit `Ctrl+C` to stop a process, you kill your entire shell connection.

---

## 🛠️ The Stabilization Ritual

### Step 1: Python PTY Spawn

Inside your limited Netcat shell, force the system to spawn a proper terminal interface:
```
python3 -c 'import pty; pty.spawn("/bin/bash")'
```

_Note: If `python3` isn't there, try `python -c ...` or `/usr/bin/script -qc /bin/bash /dev/null`._

### Step 2: Background & Raw Mode

1. **Background:** Hit `Ctrl+Z` (your shell will disappear and you'll be back on your local machine).
    
2. **Set Local Terminal to Raw:**
    ```
    stty raw -echo; fg
    ```
    
3. **Reset:** Hit `[Enter]` then `[Enter]` again. Your shell is back, but now it supports `Ctrl+C`!
    

### Step 3: Environment Configuration

Make the shell look pretty and support clear screens:
```
export TERM=xterm-256color
```

### Step 4: Fix Screen Dimensions (Optional but Recommended)

If your text is wrapping weirdly or `nano/vim` looks small, match the remote rows/columns to your local window.

1. **On Local Machine (New Terminal):** Run `stty size` (e.g., it says `44 180`).
    
2. **In the Remote Shell:**
    ```
    stty rows 44 columns 180
    ```
    

---

## 📊 Why This Matters

|**Feature**|**Dumb NC Shell**|**Upgraded TTY Shell**|
|---|---|---|
|**Tab Completion**|❌ No|✅ Yes|
|**Ctrl+C Safety**|❌ Connection Dies|✅ Process Stops Only|
|**Arrow Keys**|❌ `^[[A`|✅ History Browsing|
|**Editors (Vim)**|❌ Broken UI|✅ Works Perfectly|
