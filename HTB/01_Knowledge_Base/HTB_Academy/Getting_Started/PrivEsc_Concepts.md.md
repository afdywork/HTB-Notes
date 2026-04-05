> **The Goal:** Elevate privileges from a low-level service/user account to a high-level administrative account.

- **Lateral Movement:** Moving from one standard user to another (e.g., `user1` to `user2`).
    
- **Vertical Movement:** Moving from a standard user to a SuperUser (e.g., `user2` to `root` or `SYSTEM`).
    
- **The Sudoers File:** A configuration file (`/etc/sudoers`) that defines which users can run which commands as other users.
    
- **Sensitive Files:** Users often leave "keys to the kingdom" in plain sight. Always check `.ssh`, `.bash_history`, and `config` files.