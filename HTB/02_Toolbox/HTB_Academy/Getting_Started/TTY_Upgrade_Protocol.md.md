Standard Netcat shells are "dumb" (no arrow keys, no tab autocomplete, `Ctrl+C` kills the session). Use this ritual to fix it:

1. **Inside the NC shell:**
    
    Bash
    
    ```
    python3 -c 'import pty; pty.spawn("/bin/bash")'
    ```
    
2. **Background the shell:** Hit `Ctrl+Z`.
    
3. **On your local machine (Kali/Pwnbox):**
    
    Bash
    
    ```
    stty raw -echo; fg
    # Then hit [Enter] [Enter]
    ```
    
4. **Set Terminal Environment:**
    
    Bash
    
    ```
    export TERM=xterm-256color
    ```
    
5. **Fix Screen Size:** Check your local size with `stty size`. Then set it in the remote shell:
    
    Bash
    
    ```
    stty rows [X] columns [Y]
    ```