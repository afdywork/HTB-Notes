## 🔍 The 6 Port States

|**State**|**Meaning**|**Technical Reason**|
|---|---|---|
|**Open**|Connection established.|TCP: SYN-ACK received|
|**Closed**|No service listening.|Target sent **RST** flag.|
|**Filtered**|Communication blocked.|No response (Dropped) or ICMP error (Rejected).|
|**Unfiltered**|Accessible, state unknown.|Only occurs during ACK scans (`-sA`).|
|**Open\|Filtered**|Open or firewall blocked.|No response received (usually UDP or specific TCP scans).|
|**Closed\|Filtered**|Closed or firewall blocked.|Only in Idle scans.|

---

## 🛠️ The "Analyst's View" Flags

When studying how Nmap works or debugging why a scan is failing, use these flags to disable "automated" noise:

- `-Pn`: **No Ping.** Skips host discovery. Treats the target as online.
    
- `-n`: **No DNS.** Skips reverse DNS resolution (makes scans faster).
    
- `--disable-arp-ping`: Forces Nmap to use Layer 3 (IP) instead of Layer 2 (ARP) on local networks.
    
- `--packet-trace`: **Crucial.** Shows every packet SENT and RECEIVED in the terminal.
    
- `--reason`: Explicitly states _why_ Nmap marked a port as open, closed, or filtered (e.g., `syn-ack`, `port-unreach`).
    

---

## 🛠️ TCP Scanning Methods

### 1. SYN Scan (`-sS`) - The "Half-Open" Scan

The default scan (as root). It is fast and avoids completing the handshake.
```
# Basic SYN scan against top ports
sudo nmap -sS 10.129.2.28 --top-ports=10

# Analyzing a CLOSED port (shows the RST-ACK response)
sudo nmap 10.129.2.28 -p 21 --packet-trace -Pn -n --disable-arp-ping
```

### 2. Connect Scan (`-sT`) - The "Polite" Scan

Completes the full 3-way handshake. Used when the user doesn't have raw socket privileges.
```
# Analyzing an OPEN port (shows 'Connected' in trace)
sudo nmap 10.129.2.28 -p 443 --packet-trace --disable-arp-ping -Pn -n --reason -sT
```

---

## 🛠️ UDP Scanning (`-sU`)

UDP is stateless. If no response is received, Nmap can't tell if the port is open or just dropped by a firewall.
```
# Scan top 100 UDP ports
sudo nmap -sU -F 10.129.2.28

# Analyzing an OPEN UDP port (requires a response from the app)
sudo nmap 10.129.2.28 -sU -p 137 -Pn -n --disable-arp-ping --packet-trace --reason
```

---

## 🧬 Decoding the "Filtered" Logic

A firewall can handle your "knocking" in two distinct ways:

### 1. The "Drop" (Slow)

The firewall simply ignores the packet. Nmap retries (`--max-retries`) and then gives up.
```
# Notice the delay (~2s) and multiple SENT packets with no RCVD
sudo nmap 10.129.2.28 -p 139 --packet-trace -n --disable-arp-ping -Pn
```

### 2. The "Reject" (Fast)

The firewall actively tells you "No" using an ICMP error.
```
# Result: RCVD ICMP [Port unreachable (type=3/code=3)]
sudo nmap 10.129.2.28 -p 445 --packet-trace -n --disable-arp-ping -Pn
```

---

## 🛠️ Service & Version Detection (`-sV`)

Probes open ports to determine the exact service and version. This is the bridge between "finding a door" and "finding a way in."
```
# Example: Identifying an Ubuntu-based Samba service
sudo nmap 10.129.2.28 -p 445 -sV -Pn -n --disable-arp-ping --packet-trace --reason
```