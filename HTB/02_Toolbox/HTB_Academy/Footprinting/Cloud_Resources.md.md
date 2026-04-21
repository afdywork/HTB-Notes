# Cloud Resource Footprinting

**Module:** #Footprinting
**Location:** `02_Toolbox/HTB_Academy/04_Footprinting/04_Cloud_Resources.md`

🎯 **Objective**

Identify misconfigured or publicly accessible cloud storage (AWS S3, Azure Blobs, GCP Buckets) and leaked sensitive data (SSH keys, credentials) through passive discovery.

🔍 **The Concept**

Cloud providers secure the _infrastructure_, but administrators are responsible for _configuration_. Mistakes in permissions often lead to unauthenticated access to sensitive data storage.

---

### 🛠️ Discovery Techniques

#### 1. DNS Analysis & IP Lookups

Administrators often create DNS CNAME records for cloud buckets to make them easier for employees to remember.
```
# Identifying cloud-hosted servers in your subdomain list
for i in $(cat subdomainlist); do host $i | grep "has address" | grep inlanefreight.com | cut -d" " -f1,4; done
```

- **Indicator:** Look for addresses like `s3-website-us-west-2.amazonaws.com` or `10.129.x.x` (if part of a known cloud range).
    

#### 2. Google Dorking

Use specific search operators to find indexed files on cloud platforms.

- **AWS:** `site:s3.amazonaws.com intext:"company_name"`
    
- **Azure:** `site:blob.core.windows.net intext:"company_name"`
    
- **General:** `inurl:company_name + "password"`, `inurl:company_name + "secret"`
    

#### 3. Source Code Inspection

Images, JavaScript, and CSS are often offloaded to cloud buckets to save web server resources.

- **Action:** View the page source (`Ctrl+U`) and search for strings like `s3`, `azure`, or `googleapis`.
    

---

### 🌐 Essential Third-Party Tools

|**Tool**|**Purpose**|**Intelligence Gained**|
|---|---|---|
|**Domain.Glass**|Domain/Infrastructure profiling.|Reveals Nameservers, SPF records, and security status (e.g., Cloudflare usage).|
|**GrayHatWarfare**|Cloud storage search engine.|Search and filter public AWS, Azure, and GCP buckets by company name or file type (e.g., `.pdf`, `.zip`, `.txt`).|

---

### ⚠️ High-Value Findings (The "Crown Jewels")

When exploring public cloud buckets, prioritize finding:

- **SSH Private Keys (`id_rsa`):** Allows passwordless access to company servers.
    
- **Configuration Files (`web.config`, `.env`):** Often contain database credentials or API keys.
    
- **Internal Documentation:** PDFs or presentations detailing internal network structure or employee rosters.
    
- **Backup Files (`.bak`, `.sql`):** Potential for massive data leaks or credential harvesting.