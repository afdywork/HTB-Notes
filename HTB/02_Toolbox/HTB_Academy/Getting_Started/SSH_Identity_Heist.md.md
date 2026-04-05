_The exact workflow you used to win today._

1. **Find the Key:** `cat /root/.ssh/id_rsa`
    
2. **Copy/Paste** to a local file named `id_rsa`.
    
3. **Fix Permissions (The Golden Rule):**
    
    Bash
    
    ```
    chmod 600 id_rsa
    ```
    
4. **Login:**
    
    Bash
    
    ```
    ssh [user]@[IP] -i id_rsa -p [PORT]
    ```