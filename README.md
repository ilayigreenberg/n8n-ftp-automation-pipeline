# Multi-Container Local n8n Automation & FTP Data Pipeline

An end-to-end local microservice and automation pipeline # built as part of a DevOps & Automation assignment. 

The pipeline ingests food and location search parameters via a public webhook endpoint (exposed through ngrok), fetches
real-time restaurant data using SerpApi, cleanses and normalizes the JSON payload using custom JavaScript logic, and automatically provisions a structured JSON file onto an isolated local FTP server.

---

## System Architecture

1. **Ingress Layer (ngrok):** Exposes the local n8n webhook listener to public HTTP POST requests.
2. **Orchestration Layer (n8n in Docker):** Handles workflow execution, API requests, and data processing.
3. **External API (SerpApi / Google Local Results):** Discovers top-matching culinary locations dynamically based on user input.
4. **Data Normalization Engine (Code Node):** Parses free-form text operating hours, handles complex split-shift schedules, and normalizes schema structures.
5. **Storage Layer (vsftpd FTP Server in Docker):** Receives and persists dynamically named output `.json` files inside an isolated volume.

---

## Project Structure

```text
.
├── docker-compose.yml       # Orchestration manifest for n8n & vsftpd services
├── workflow.json            # Exported production-ready n8n workflow pipeline
├── sample_output.json       # Validated FTP data payload ("Burgers in London")
└── README.md                # Project documentation
