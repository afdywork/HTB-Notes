**Category:** #Utilities / #Editing
**Scope:** Linux / Unix

---

## 🔍 Overview

> **Logic:** Vim is the "Sword of the System Administrator." You will find it on almost every Linux target. Knowing how to exit it without panicking is a mandatory skill for any hacker.

---

## ⌨️ Essential Commands

|**Mode / Action**|**Key**|**Description**|
|---|---|---|
|**Insert Mode**|`i`|Start typing/editing text.|
|**Command Mode**|`Esc`|Return to command mode (stop typing).|
|**Save & Exit**|`:wq`|**W**rite to disk and **Q**uit.|
|**Force Quit**|`:q!`|Exit **without** saving (The "Panic" button).|
|**Delete Line**|`dd`|Deletes the current line.|

---

## 🚀 Pro-Tips for Hackers

### 🏗️ Creating a New File

To create a quick exploit script or a `.gitignore` file:
```
vim filename.txt
```

### 🔍 Searching within Vim

In **Command Mode** (press `Esc` first):

- Type `/word` and press **Enter** to search for a specific string.
    
- Press `n` to go to the next match.
    

### 📋 Line Numbers

If you are debugging code, type this in Command Mode:
```
:set number
```