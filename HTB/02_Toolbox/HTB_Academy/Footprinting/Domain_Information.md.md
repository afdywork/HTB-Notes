# Domain Reconnaissance (Passive)

**Module:** #Footprinting
**Location:** `02_Toolbox/HTB_Academy/04_Footprinting/03_Domain_Information.md`

🎯 **Objective**

Gather infrastructure data (domains, subdomains, third-party providers) without direct interaction with the target's systems to maintain a low profile.

🔍 **Key Principles**

- **Passive Only:** Use third-party logs and services (Certificate Transparency, Shodan, DNS).
    
- **The Developer View:** Analyze the company's website text to understand what technologies (IoT, App Dev, Data Science) they likely use.
    
- **Invisible Assets:** Look for verification codes in DNS records that reveal the company's "hidden" cloud ecosystem (Atlassian, Google, Outlook, etc.).
    

---

### 🛠️ Subdomain Discovery (Certificate Transparency)

SSL/TLS certificates are logged publicly. We can query these logs to find subdomains that might not be linked on the main site.

**Using crt.sh (JSON & CLI Filtering):**
```
# Get raw JSON output for a domain
curl -s https://crt.sh/\?q\=inlanefreight.com\&output\=json | jq .

# Filter for a clean list of unique subdomains
curl -s https://crt.sh/\?q\=google.com\&output\=json | jq . | grep name | cut -d":" -f2 | grep -v "CN=" | cut -d'"' -f2 | awk '{gsub(/\\n/,"\n");}1;' | sort -u
```

---

### 🔍 Infrastructure Analysis

Once subdomains are found, we need to map them to IP addresses and identify which ones are directly managed by the company versus third-party hosts (like AWS/Cloudflare).

**Resolving Subdomains to IPs:**
```
# Assuming you have a file named 'subdomainlist'
for i in $(cat subdomainlist); do host $i | grep "has address" | grep inlanefreight.com | cut -d" " -f1,4; done
```

**Shodan Host Analysis:**

Query Shodan for these IPs to see open ports and service banners without scanning them yourself.
```
# Create an IP list first
for i in $(cat subdomainlist); do host $i | grep "has address" | grep inlanefreight.com | cut -d" " -f4 >> ip-addresses.txt; done

# Query Shodan for each IP
for i in $(cat ip-addresses.txt); do shodan host $i; done
```

---

### 🧬 DNS Record Deep-Dive

DNS records often contain "breadcrumbs" about the company's internal operations.

**Command:**
```
dig any inlanefreight.com
```

**What to look for:**

|**Record Type**|**Intelligence Gained**|
|---|---|
|**A**|Direct IP mapping to the domain.|
|**MX**|Mail provider (Google, Outlook). Suggests potential for GDrive/OneDrive leaks.|
|**NS**|Nameservers. Identifies the hosting provider (e.g., INWX, AWS).|
|**TXT**|**Goldmine.** Look for verification codes for Atlassian (Jira/Confluence), Mailgun (APIs), and SPF/DMARC records (leaks internal IP ranges).|

---

### 📋 Third-Party Indicators in TXT Records

If you see these in `dig any` results, note them for future attack vectors:

- **Atlassian:** Potential for Jira/Confluence exploits or leaked credentials.
    
- **LogMeIn:** Remote management access; target for credential stuffing.
    
- **Mailgun:** Indicates API usage; look for IDOR or SSRF on API endpoints.
    
- **v=spf1:** Reveals internal IP addresses/netblocks authorized to send mail.