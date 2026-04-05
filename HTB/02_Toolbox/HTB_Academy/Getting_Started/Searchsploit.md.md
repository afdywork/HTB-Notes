**Syntax:** `searchsploit [options] [term]` **Commands:**

# Install the exploit database locally

sudo apt install exploitdb -y

# Search for exploits by application name and version

searchsploit openssh 7.2

# Search for a specific CVE

searchsploit --cve 2021-3156

# Copy an exploit to your current directory (use the Path from search results)

searchsploit -m linux/remote/45233.py

# Examine the contents of an exploit without downloading

searchsploit -x linux/remote/45233.py

**Flags/Parameters:**

- **-m:** Mirror (copies) the exploit to your current working directory.
    
- **-x:** Examines (opens) the exploit file in your terminal pager.
    
- **-w:** Shows the URL to the Exploit-DB online version.
    
- **--cve:** Filters results specifically for a Common Vulnerabilities and Exposures ID.