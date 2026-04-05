**Banner Grabbing:** A technique used to gain information about a remote computer system on a network and the services running on its open ports.

- **Why it works:** Many servers (like Apache, SSH, or FTP) are "polite"—when you connect, they send a greeting that includes their name and version number.
    
- **The Risk:** In a real-world hardened environment, admins often disable these banners to make it harder for attackers to find specific exploits for that version.