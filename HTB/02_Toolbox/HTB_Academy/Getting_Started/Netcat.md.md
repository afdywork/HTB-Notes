**Syntax:** `nc [flags] [target] [port]`

**Commands:**
```
# Banner Grabbing
# Manually connects to a port to identify the service/version.
nc [IP] [port]

# Setting up a Listener
# Sets up a local port to wait for an incoming connection.
nc -lvnp [port]

# Manual Banner Grab (Example: FTP)
nc -nv 10.129.42.253 21
```

**Flags/Parameters:**
- `-l`: **Listen** mode (waits for a connection).
    
- `-v`: **Verbose** (shows details of the connection).
    
- `-n`: **No DNS** resolution (faster, avoids creating DNS logs).
    
- `-p`: **Port** (specifies the port number).