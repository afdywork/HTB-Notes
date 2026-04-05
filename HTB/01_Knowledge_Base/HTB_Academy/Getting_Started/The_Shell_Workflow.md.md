- **Enumeration:** You find a website (Tom).
    
- **Discovery:** You find a "File Upload" button or a "Search Bar" that is broken.
    
- **Delivery:** * _Web Shell:_ You upload `shell.php`.
    
    - _Reverse Shell:_ You type the `bash -i` string into the search bar.
        
- **Interaction:** * If you used a **Web Shell**, you go to `site.com/shell.php?cmd=whoami`.
    
    - If you used a **Reverse Shell**, you check your `nc -lvnp` listener to see if Tom called you back.