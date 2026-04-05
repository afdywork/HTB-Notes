**Syntax:** `ftp [flags] [target]`

**Commands:**
```
# Connect to FTP (Passive mode)
ftp -p 10.129.42.253

# Commands inside FTP prompt:
ls              # List files
cd [dir]        # Change directory
get [file]      # Download a file
exit/bye        # Disconnect
```

**Key Finding:** Look for `anonymous` login. If allowed, use `anonymous` as the username and any password.