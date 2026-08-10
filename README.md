# Multi-Container Local n8n Automation & FTP Data Pipeline

An end-to-end local microservice and automation pipeline. 

The pipeline ingests food and location search parameters via a public webhook endpoint (exposed through ngrok), fetches
real-time restaurant data using SerpApi, cleanses and normalizes the JSON payload using custom JavaScript logic, and automatically provisions a structured JSON file onto an isolated local FTP server.

---

## System Architecture

1. **Ingress Layer (ngrok & Webhook):** Accepts incoming HTTP POST requests from external clients through a secure ngrok tunnel and routes them to the local n8n Webhook node.
2. **Orchestration Layer (n8n in Docker):** Manages pipeline execution, data transformations, and service interaction within an isolated Docker environment.
3. **Data Harvesting & Egress Layer (SerpApi):** Discovers top-matching culinary locations dynamically based on user input.
4. **Data Normalization Engine (Code Node):** Extracts the top 10 results, maps daily operating hours objects, applies fallback values for missing parameters (e.g., phone numbers), and restructures the payload into the required JSON schema.
5. **Storage Layer (vsftpd FTP Server in Docker):** Persists normalized target `.json` files directly onto a local volume managed by an independent vsftpd container.

---

## Project Structure

```text
.
├── compose.yml                    # Orchestration manifest for n8n & vsftpd services
├── restaurant-pipeline.json       # Exported production-ready n8n workflow pipeline
├── sample_output.json             # Validated FTP data payload ("Burgers in London")
└── README.md                      # Project documentation
```

---

## Prerequisites

- Docker & Docker Compose V2
- Ubuntu 24.04 LTS (WSL2 / VM environment)
- ngrok account & CLI configured

---

## Getting Started

#### 1. Clone the Repository
`git clone [https://github.com/ilayigreenberg/n8n-ftp-automation-pipeline.git](https://github.com/ilayigreenberg/n8n-ftp-automation-pipeline.git)
cd n8n-ftp-automation-pipeline`
#### 2. Launch Container Stack
Spin up the n8n and FTP containers defined in compose.yml: `docker compose up -d`  
Verify that all services are running cleanly: `docker compose ps`
#### 3. Expose the Webhook
In a separate terminal, expose port *5678* using ngrok: `ngrok http 5678`
#### 4. Import Workflow to n8n
   1. Open your browser and navigate to *http://localhost:5678*.
   2. Import the *restaurant-pipeline.json* file.
   3. Activate the workflow (*Publish*)
