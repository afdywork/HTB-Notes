**Module:** #Network_Enumeration_with_Nmap
**Path:** `02_Toolbox/HTB_Academy/02_Network_Enumeration_with_Nmap/Saving_Results.md`

---

## 🛠️ The Output Formats

Nmap allows you to save results in three distinct formats depending on how you plan to use the data:

|**Flag**|**Format Type**|**Extension**|**Best Use Case**|
|---|---|---|---|
|**`-oN`**|**Normal**|`.nmap`|Standard readable output; looks exactly like the terminal screen.|
|**`-oG`**|**Grepable**|`.gnmap`|One-line-per-host format; perfect for searching with `grep`, `awk`, or `cut`.|
|**`-oX`**|**XML**|`.xml`|Structured data; used for generating HTML reports or importing into databases.|
|**`-oA`**|**All Formats**|`.*`|**Recommended.** Saves in all three formats simultaneously using the provided filename.|

### 🚀 Usage Examples
```
# Save only a grepable file
sudo nmap 10.129.2.28 -p 22,80 -oG web_ssh_only

# Save everything (Standard Practice)
sudo nmap 10.129.2.28 -p- -oA full_scan_results
```

---

## 🎨 Professional HTML Reporting

The XML output (`-oX`) is the "raw material" for professional documentation. You can transform it into a clean, interactive HTML page.

### 🛠️ The Conversion Workflow

1. **Generate XML:** Ensure your scan included `-oX` or `-oA`.
    
2. **Process with XSLT:** Use the `xsltproc` tool to apply the Nmap stylesheet.
    ```
    xsltproc target.xml -o target.html
    ```
    
3. **View:** Open the `.html` file in any browser to see a structured table of ports, services, and versions.
    

---

## 🧬 Why Grepable (`-oG`) Matters

In large-scale engagements with hundreds of targets, you can't read every `.nmap` file. The `.gnmap` format allows you to find targets instantly:
```
# Example: Find all IP addresses that have port 80 OPEN in your results
grep "80/open" target.gnmap | cut -d" " -f2
```