# Staff & Professional OSINT

**Module:** #Footprinting
**Location:** `01_Knowledge_Base/OSINT/Staff_and_Professional_Recon.md`

🎯 **Objective**

Identify a company's internal technology stack, security posture, and potential vulnerabilities by analyzing the professional profiles and digital footprints of its employees.

🔍 **The Strategy**

Infrastructure isn't just hardware; it's a reflection of the people who build it. By studying employees on **LinkedIn**, **Xing**, and **Job Boards**, we can "reverse-engineer" the corporate network without sending a single packet to the target.

---

### 📋 Information Sources & Intel Yield

#### 1. Job Postings (The "Blueprints")

Job descriptions are often accidental disclosures of a company's entire stack.

- **Languages:** e.g., Python, Java, Ruby (tells you what exploits to research).
    
- **Databases:** e.g., PostgreSQL, Oracle (helps narrow down SQLi payloads).
    
- **Frameworks:** e.g., Django, Flask, ASP.NET (reveals common file structures like `settings.py` or `web.config`).
    
- **DevOps/Tools:** Mention of **Jira, Bitbucket, or Git** confirms the existence of Atlassian resources identified in Layer 1 (Internet Presence).
    
- **Security Requirements:** Mentions of **Security+**, **Docker/K8s**, or **SIEM** tools tell you the level of defensive maturity.
    

#### 2. Employee Profiles (The "Technical Maps")

- **Skills/Certifications:** High concentrations of AWS/Azure certified staff suggest a heavy cloud footprint.
    
- **"About" Sections:** Employees often brag about specific projects (e.g., "Migrated entire database to PostgreSQL"). This confirms the active use of specific versions or technologies.
    
- **Career History:** If a lead engineer spent years at a security firm, expect a harder target. If they are juniors with no security background, expect more misconfigurations.
    

#### 3. GitHub & Personal Repositories

Employees often mirror professional work or "practice" projects that mimic company infrastructure.

- **The Danger:** Hardcoded JWT tokens, API keys, or personal email addresses found in commit histories.
    
- **Naming Conventions:** Studying an employee's Django project on GitHub reveals how they likely name files and directories on the company's production server.
    

---

### 🛠️ Strategic Questions to Ask

- **What is the "Security DNA" of the team?** (Do they prioritize security or speed?)
    
- **Which technologies are "Legacy" vs "New"?** (Older tech = more unpatched vulnerabilities; Newer tech = more misconfigurations).
    
- **What third-party providers are mentioned?** (LogMeIn, Mailgun, Slack).
    

---

### 📋 Intel Extraction Example

|**Discovery**|**Inferred Technical Target**|
|---|---|
|Job mentions "Django/Flask"|Look for `DEBUG=True` errors or OWASP Top 10 Django flaws.|
|Engineer lists "PostgreSQL"|Focus on port `5432` and default credentials.|
|HR lists "Atlassian Suite"|Search for public Jira dashboards or Confluence pages.|
|Employee repo has a JWT token|Test if that token is reused across company subdomains.|