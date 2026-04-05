**Category:** #Utilities / #Persistence
**Scope:** Linux / Terminal Management

---

## 🔍 Overview

> **Logic:** Tmux allows you to run multiple terminal windows inside a single session. More importantly, it keeps those sessions alive on the server even if you disconnect from your VPN or close your laptop.

---

## ⌨️ The "Prefix" Key

In Tmux, almost every command starts with the **Prefix**: `Ctrl + b`.

_You press them together, release, and then hit the next key._

### 🛠️ Session Management

|**Action**|**Command**|
|---|---|
|**Start New Session**|`tmux`|
|**List All Sessions**|`tmux ls`|
|**Attach to Last Session**|`tmux a`|
|**Attach to Named Session**|`tmux a -t [name]`|
|**Kill Session**|`tmux kill-session -t [name]`|

---

### 🪟 Window & Pane Control

_After pressing `Ctrl + b`:_

- **`%`** — Split screen **Vertically** (Side-by-side).
    
- **`"`** — Split screen **Horizontally** (Top and bottom).
    
- **`o`** — Cycle through panes.
    
- **`x`** — Close the current pane.
    
- **`d`** — **Detach** (Leave the session running in the background).
    

---

## 🚀 The "Hacker" Layout Strategy

A common pro-layout for an HTB box looks like this:

1. **Left Pane:** Your active exploitation (Nmap, GoBuster, or Python exploit).
    
2. **Right Top Pane:** Your `nc -lvnp` listener (Waiting for the shell).
    
3. **Right Bottom Pane:** A "Work Log" or `tail -f` on a log file.