**Module:** #Network_Enumeration_with_Nmap 
**Category:** #Reconnaissance / #Scanning

## 🛠️ 1. Scanning Network Ranges & Lists

The `-sn` flag is the key here. It disables port scanning to focus purely on finding "Live" hosts.

### A. Scan an Entire Subnet (/24)

This command scans 254 IPs, saves all formats to files named `tnet`, and filters the output to show only the IP addresses.
```
sudo nmap 10.129.2.0/24 -sn -oA tnet | grep for | cut -d" " -f5
```

### B. Scan From an Input List (`-iL`)

Used when you are given a specific scope of IPs in a text file (e.g., `hosts.lst`).
```
sudo nmap -sn -oA tnet -iL hosts.lst | grep for | cut -d" " -f5
```

### C. Scan Multiple Specific IPs or Ranges
```
# Individual IPs
sudo nmap -sn -oA tnet 10.129.2.18 10.129.2.19 10.129.2.20

# Range within an octet
sudo nmap -sn -oA tnet 10.129.2.18-20
```

---

## 🛠️ 2. Deep Discovery & Packet Analysis

When a simple scan isn't enough, we use these commands to see **why** a host is responding.

### A. Basic Single Host Discovery
```
sudo nmap 10.129.2.18 -sn -oA host
```

### B. ICMP Echo Request + Packet Trace

This forces an ICMP Ping (`-PE`) and shows the actual packets sent/received (`--packet-trace`). Note that on a local network, Nmap will still use **ARP** first.
```
sudo nmap 10.129.2.18 -sn -oA host -PE --packet-trace
```

### C. The "Reason" Scan

Adds a column to the output explaining why Nmap thinks the host is up (e.g., `received arp-response`).
```
sudo nmap 10.129.2.18 -sn -oA host -PE --reason
```

### D. Bypassing ARP (The "True" ICMP Test)

The final form of the command used in the module. This disables ARP pings to force a pure ICMP interaction, allowing you to see the **TTL** values.
```
sudo nmap 10.129.2.18 -sn -oA host -PE --packet-trace --disable-arp-ping
```

---

## 🔍 The TTL Fingerprint Table (Quick Reference)

From the `--packet-trace` output, look for the `ttl` value to identify the OS:

|**TTL Value**|**Likely Operating System**|
|---|---|
|**64**|Linux / FreeBSD / macOS|
|**128**|**Windows** (Our target in Section 3)|
|**254/255**|Cisco / Network Infrastructure|