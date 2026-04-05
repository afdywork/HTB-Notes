_This is your "Manual Inspection" list for when automated scripts fail._

#### **1. Quick Identity Check**

Bash

```
whoami                      # Current username
id                          # Groups and UID/GID
sudo -l                     # List allowed sudo commands
```

#### **2. Searching for SUID Files**

_Finds files that run with the owner's (Root) privileges._

Bash

```
find / -perm -4000 2>/dev/null
```

#### **3. Finding Writable Directories**

_Where can I upload my tools?_

Bash

```
find / -writable -type d 2>/dev/null
```

#### **4. Searching for Passwords in Configs**

Bash

```
grep -rni "password" /var/www/html/ 2>/dev/null
```