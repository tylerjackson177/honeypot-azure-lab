# 🧪 Azure Honeypot Lab with DVWA, Logging & IDS

This project sets up a vulnerable **Windows 10 honeypot** hosted in **Azure**, integrated with:
- 💥 [DVWA (Damn Vulnerable Web Application)](https://github.com/digininja/DVWA)
- 🔐 Azure Network Security Groups (NSG)
- 📊 Log Analytics + Microsoft Sentinel (SIEM)
- 🌍 Geo-IP enrichment via Sentinel Watchlists
- 🛡️ Lightweight IDS for detection simulation

The lab is designed to simulate **real-world attacks** (e.g., brute force, SQL injection), provide hands-on experience with **detection engineering**, and enhance skills in **SIEM integration and cloud threat hunting**.

---

## 🎯 Objectives

- ✅ Deploy vulnerable Azure VM with **DVWA** and exposed RDP/HTTP ports
- ✅ Forward Windows Security logs to **Azure Sentinel**
- ✅ Build **KQL queries** to detect brute-force login attempts (Event ID 4625)
- ✅ Enrich logs with **geo-IP location data** using a CSV-based watchlist
- ✅ Visualize attacks in a **custom Sentinel Workbook**
- ✅ Log and analyze **SQL injection attempts** in DVWA
- ✅ Install a basic **IDS** and simulate alert correlation

---

## 🛠️ Lab Setup

### 🔧 Part 1: Azure Subscription
- Created Azure subscription and deployed resources via [Azure Portal](https://portal.azure.com)

### 💻 Part 2: Deploy Honeypot VM
- Deployed **Windows 10 VM**
- Installed and configured **XAMPP + DVWA**
- Exposed ports: `3389` (RDP), `80` (HTTP)
- Disabled Windows Firewall for visibility

### 🔍 Part 3: Simulated Attacks
- Performed failed logins as fake users (`employee`, `admin`, `svc`)
- Observed **Event ID 4625** in **Event Viewer**
- Ran **SQLi payloads** in DVWA (logged manually to `sqli.log`)

### 📤 Part 4: Log Forwarding to Sentinel
- Created **Log Analytics Workspace (LAW)**
- Connected Sentinel + enabled **Security Events via AMA**
- Created **DCR** to forward 4624/4625 events

### 🌐 Part 5: Geo-IP Enrichment
- Uploaded `geoip-summarized.csv` as a **Sentinel Watchlist**
- Used `ipv4_lookup()` in KQL to enrich attacker IPs with country/ASN

### 🗺️ Part 6: Attack Map
- Built a **Sentinel Workbook** visualizing attacker geolocation + frequency

---

## 🧠 Sample KQL Query

```kql
let GeoIPDB = _GetWatchlist("geoip");
SecurityEvent
| where EventID == 4625
| extend IP = tostring(parse_json(AdditionalFields)["IpAddress"])
| evaluate ipv4_lookup(GeoIPDB, IP, network)
| summarize FailedLogons = count() by IP, Country, Account, bin(TimeGenerated, 1h)
