# 🛡️ Automated SOC Threat Intelligence Pipeline

![n8n](https://img.shields.io/badge/n8n-Workflow_Automation-FF6560?style=for-the-badge)
![JavaScript](https://img.shields.io/badge/JavaScript-ES6+-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)
![Security](https://img.shields.io/badge/SecOps-Threat_Intel-blue?style=for-the-badge)

A resilient, fault-tolerant **SOAR (Security Orchestration, Automation, and Response)** workflow built with n8n. This project automates the manual labor of investigating network alerts by ingesting raw IPs, enriching them with threat intelligence, and routing them based on a custom risk scoring engine.

> **Objective:** Demonstrate the ability to build enterprise-grade security automation that balances speed (alerting) with compliance (logging), featuring robust error handling and parallel processing.

## 🧠 Architecture Logic

This workflow moves beyond simple linear execution by utilizing **parallel branching** to ensure logging failures do not block critical security alerts.

1. **Ingest:** Receives JSON payload from a SIEM or Firewall (simulated for demo).
2. **Enrich:** Queries **AbuseIPDB API** to fetch ISP, Country, and Threat Confidence scores.
3. **Score:** Calculates a normalized risk score (0-100) using custom JavaScript logic.
4. **Fork (Parallel Execution):**
    * **Path A (Audit):** Logs 100% of traffic to Google Sheets for compliance.
    * **Path B (Alert):** Filters for High-Risk threats and sends instant Telegram notifications.
5. **Fail-Safe:** A dedicated Error Trigger watches for API timeouts and alerts system admins immediately.

## ✨ Key Features

* **⚡ Parallel Processing:** Decoupled the *Audit Logging* (Google Sheets) from the *Alerting* (Telegram). If the database API fails, the critical security alert is still delivered.
* **🛡️ Dead Letter Queue (Error Handling):** Implemented a standalone Error Trigger node. If any part of the pipeline crashes (e.g., API timeout), a separate "System Failure" alert is sent to the admin with the specific Execution ID for debugging.
* **📊 Automated Enrichment:** Integrated **AbuseIPDB API** to fetch real-time ISP, Country, and Confidence Scores for incoming IP addresses.
* **🧮 Custom Scoring Logic:** JavaScript-based node that normalizes external threat data into a simplified internal risk score (0-100) and assigns a verdict (`SAFE`, `SUSPICIOUS`, `HIGH_RISK`).
* **📈 Visualization Dashboard:** Data is piped to Google Sheets, which powers a **Looker Studio Dashboard** for real-time threat mapping.

## 🛠️ Tech Stack

* **Orchestration:** [n8n](https://n8n.io/)
* **Scripting:** JavaScript (Node.js environment)
* **Threat Intel:** AbuseIPDB API
* **Database/Audit:** Google Sheets
* **Notification:** Telegram Bot API
* **Visualization:** Google Looker Studio

## 📸 Screenshots

### 1. The Workflow (Parallel Execution)
*Notice the "Fork" after the Score node—logging and alerting happen simultaneously.*
![Workflow Canvas](images/image_46d4d8.png)

### 2. The Dashboard (Threat Map)
*Real-time visualization of blocked attacks by country.*
*(Add your Looker Studio screenshot here)*

## 🚀 Quick Start (Import Workflow)

1.  **Install n8n:**
    * Self-hosted: `npm run n8n` or `docker run -it --rm --name n8n -p 5678:5678 -v ~/.n8n:/home/node/.n8n n8n-io/n8n`
    * Cloud: [Sign up for n8n Cloud](https://n8n.io/)
2.  **Import Code:**
    * Download `SOC_Automation_IP_Enrichment.json` from this repo.
    * In n8n, click **Workflow** → **Import from File**.
3.  **Configure Credentials:**
    * **AbuseIPDB:** Get a free API key and add it to the HTTP Request node (Header Auth).
    * **Telegram:** Create a bot via `@BotFather` and add credentials.
    * **Google Sheets:** Connect your Google Service Account or OAuth.
4.  **Test:**
    * Click "Execute Workflow". The `Normalize` node contains test data to simulate an attack.

## 🔮 Future Roadmap

* [ ] **Two-Way Sync:** Add buttons to the Telegram alert ("Block IP" / "Ignore") that trigger a callback to the firewall.
* [ ] **Rate Limiting:** Implement Redis to cache API results and save AbuseIPDB credits.
* [ ] **Multi-Source Intel:** Add VirusTotal and Shodan for deeper context on hash/domain indicators.

---

**Author:** Sumitpal
**License:** MIT
