# Nmap: Performance & Timing

**Module:** #Network_Enumeration_with_Nmap
**Location:** `02_Toolbox/HTB_Academy/02_Network_Enumeration_with_Nmap/Nmap_Performance_Optimization.md`

---

## ⚖️ The Core Trade-off

Optimizing Nmap performance is a balancing act. If you scan too fast, you risk **dropped packets**, which leads to **False Negatives** (missing open ports).

### 1. RTT (Round-Trip-Time) Timeouts

Nmap measures how long it takes for a response to return. You can force these limits to speed up scans on slow networks.

- **`--initial-rtt-timeout <time>`**: The starting wait time (default 100ms).
    
- **`--max-rtt-timeout <time>`**: The absolute ceiling for waiting on a response.
    

> [!EXAMPLE]
> 
> `sudo nmap 10.129.2.0/24 -F --initial-rtt-timeout 50ms --max-rtt-timeout 100ms`
> 
> - _Result:_ Scans 4x faster, but may miss hosts if the network has a sudden lag spike.
>     

### 2. Max Retries

By default, Nmap retries a port **10 times** if it doesn't get a response. In a lab/HTB environment, this is overkill.

- **`--max-retries <number>`**: Setting this to `0` or `1` significantly boosts speed.
    

### 3. Packet Rates

This is the "gas pedal" of Nmap.

- **`--min-rate <number>`**: Forces Nmap to send at least X packets per second.
    

---

## 🕒 Timing Templates (`-T`)

Instead of manually tweaking every setting, Nmap provides six templates.

|**Template**|**Name**|**Use Case**|
|---|---|---|
|**`-T 0`**|**Paranoid**|Used to bypass IDS (sends one packet every few minutes).|
|**`-T 1`**|**Sneaky**|Very slow, used for evasion.|
|**`-T 2`**|**Polite**|Reduces bandwidth usage; less likely to crash fragile devices.|
|**`-T 3`**|**Normal**|**The Default.** Balanced.|
|**`-T 4`**|**Aggressive**|**Recommended for CTFs/HTB.** Fast and reliable on modern networks.|
|**`-T 5`**|**Insane**|Extremely fast. Risk of missing ports or crashing services.|

---

## 🛠️ Performance Summary Table

|**Option**|**Command**|**Description**|
|---|---|---|
|**Top Ports**|`-F`|Scans only the top 100 most common ports.|
|**Min Rate**|`--min-rate <num>`|Sends a minimum of `<num>` packets per second.|
|**Max Retries**|`--max-retries <num>`|Limits the number of times Nmap re-probes a port.|
|**Aggressive**|`-T 4`|Speeds up the scan by optimizing delays and timeouts.|