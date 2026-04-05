**Syntax:** `smbclient [flags] \\\\[IP]\\[Share]`

**Commands:**
```
# List available shares (No password)
smbclient -N -L \\\\10.129.42.253

# Connect to a specific share as a user
smbclient -U bob \\\\10.129.42.253\\users

# Commands inside SMB prompt:
ls              # List directory contents
cd [dir]        # Move into a folder
get [file]      # Download file to your local machine
```

**Flags/Parameters:**
- `-N`: No password prompt (Anonymous/Null session).
    
- `-L`: List shares available on the target.
    
- `-U [user]`: Specify the username for authentication.