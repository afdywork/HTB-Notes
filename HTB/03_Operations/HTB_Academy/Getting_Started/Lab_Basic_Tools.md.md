**Date:** 2026-04-06
**Category:** #Initial_Access / #Enumeration
**Target IP:** `154.57.164.68`

---

## 🔍 Initial Connection (Banner Grabbing)

> **Objective:** Identify the service running on the target port and determine its version.

### 🛠️ Execution

Using Netcat to connect to the provided IP and Port:
```
nc 154.57.164.68 30792
```

### 📡 Intelligence Gathered

The service responded with a cleartext banner:
```
SSH-2.0-OpenSSH_8.2p1 Ubuntu-4ubuntu0.1
```

---

## 📊 Analysis

|**Field**|**Data**|**Significance**|
|---|---|---|
|**Service**|`OpenSSH`|Standard secure shell. High security, but potentially exploitable if old or misconfigured.|
|**Version**|`8.2p1`|Released around 2020. I should check for known CVEs (e.g., `CVE-2023-38408`).|
|**OS**|`Ubuntu-4ubuntu0.1`|Confirms the target is a **Linux (Ubuntu)** machine. Likely Focal Fossa (20.04 LTS).|