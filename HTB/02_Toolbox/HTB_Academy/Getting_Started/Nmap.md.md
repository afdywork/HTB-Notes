**Syntax:** `nmap [flags] [target]`

**Commands:**
```
# Basic scan (1,000 most common ports)
nmap 10.129.42.253

# Advanced Recon (Versions, Scripts, All Ports)
nmap -sV -sC -p- 10.129.42.253

# Scan for specific vulnerability scripts
nmap --script smb-os-discovery.nse -p445 10.10.10.40

# Aggressive Scan (OS, Version, Scripts, Traceroute)
nmap -A -p445 10.129.42.253

# Banner grabbing via Nmap script
nmap -sV --script=banner -p21 10.10.10.0/24

### ⚡ Fast Discovery (Lab/Exam Mode) 
# Use this to find all open ports quickly without version/script overhead
nmap -p- --min-rate 5000 [IP] 

# Once you find a port, scan it specifically for details
nmap -sV -sC -p [PORT] [IP]
```

**Flags/Parameters:**
- `-sV`: Service version detection (fingerprints application name/version).
    
- `-sC`: Runs default Nmap scripts to find vulnerabilities or extra info.
    
- `-p-`: Scans all 65,535 TCP ports.
    
- `--script [name]`: Runs a specific Nmap Scripting Engine (NSE) script.
    
- `-A`: Aggressive mode (Enables OS detection, version detection, script scanning, and traceroute).