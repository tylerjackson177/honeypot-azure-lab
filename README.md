# honeypot-azure-lab
# 🧪 Azure Honeypot Lab with DVWA, Logging & IDS

This project sets up a vulnerable **Windows 10 honeypot** hosted in Azure, complete with **Damn Vulnerable Web Application (DVWA)**, **event log forwarding**, **geo-IP enrichment**, and a basic **Intrusion Detection System (IDS)**. The environment is designed to simulate real-world attacks and provide hands-on experience with log analysis, threat detection, and SIEM integration using Microsoft Sentinel.

---

## 🎯 Project Objectives

- Deploy a vulnerable VM with **DVWA** and exposed RDP/web ports.
- Enable and forward **Security Event Logs**.
- Use KQL to hunt for threats in Azure Sentinel.
- Enrich logs with attacker **geographic location** using a watchlist.
- Generate an **attack map** to visualize brute-force attempts.
- Deploy a lightweight **IDS** to alert on network anomalies.
- Send **SQL injection logs** from DVWA to a custom log file to simulate web application attacks

---

## 🔧 Part 1: Azure Subscription Setup
Once subscribed, log in at:  
🔗 [https://portal.azure.com](https://portal.azure.com)

---

## 🖥️ Part 2: Deploy the Honeypot VM

1. Create a **Windows 10 VM** via Azure Virtual Machines.
2. In the **Network Security Group (NSG)**:
   - Add inbound rules to allow:
     - RDP (3389)
     - HTTP (80)
     - All traffic temporarily (for honeypot purposes)
3. On the VM:
   - Disable the Windows firewall:
   - 
---

## 🔍 Part 3: Simulate Logon Failures and Inspect Logs

1. Attempt 3+ failed logins using fake usernames
2. Log into the VM using correct credentials.
3. Open **Event Viewer → Security**:
   - Confirm entries for **Event ID 4625** (failed logon attempts)

---

## 📤 Part 4: Enable Log Forwarding to Azure Sentinel

1. Create a **Log Analytics Workspace (LAW)**
2. Deploy **Microsoft Sentinel** and link it to your LAW
3. Connect the Windows VM using the **"Windows Security Events via AMA"** connector
4. In Sentinel:
   - Configure the **Data Collection Rule (DCR)**
   - Enable event forwarding for relevant IDs


