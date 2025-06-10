# Microservice Architecture REST API Project


## Project Overview

The system allows an external client to interact only with the Client Service, which orchestrates calls to internal Business Logic and Database services. This demonstrates inter-service communication and token-based authentication.

## Architecture

The system includes three microservices communicating via HTTP:

1.  **Client Service (Port 8000)**: Entry point for external clients. Orchestrates the workflow. Requires token authentication.
2.  **Business Logic Service (Port 8001)**: Performs core data processing (e.g., text analysis), simulating a delay.
3.  **Database Service (Port 8002)**: Simulates a database, storing and retrieving data in memory.

**Request Flow (`/execute_workflow`):**
`Client → Client Service → DB Service (read) → Business Service (process) → DB Service (write) → Client Service → Client`

## Services

### Client Service (Port 8000)

* `POST /execute_workflow`: Protected endpoint to initiate the workflow.

### Business Logic Service (Port 8001)

* `POST /analyze`: Processes text data and returns analytical results.

### Database Service (Port 8002)

* `POST /store_record`: Stores data.
* `GET /retrieve_latest`: Retrieves the latest record.

*All services also have `GET /` and `GET /status` endpoints for basic info and health checks.*

## Prerequisites

* Python 3.9+
* `pip`
* Libraries: `fastapi`, `uvicorn`, `python-dotenv`, `httpx`

## Setup and Running

After cloning the Repository:
### Creating a Virtual Environment and Installing Dependencies

```bash
python -m venv venv
.\venv\Scripts\activate # Windows
# or source venv/bin/activate # macOS/Linux
pip install fastapi uvicorn python-dotenv httpx
```
### Environment Variables Configuration (.env)
Create a file .env in the project root:
```bash
APP_TOKEN="MySecureAndUniqueToken0401"
DB_SERVICE_URL="http://localhost:8002"
BUSINESS_SERVICE_URL="http://localhost:8001"
```
### Running the Microservices
Open three separate terminals.

Business Logic Service:
```bash
uvicorn business_service:app --reload --port 8001
```

Database Service:
```bash
uvicorn db_service:app --reload --port 8002
```

Client Service:
```bash
uvicorn client_service:app --reload --port 8000
```
### Usage
Open a fourth terminal for making requests.

For PowerShell:
```bash
Invoke-RestMethod -Uri "http://localhost:8000/execute_workflow" -Method Post -Headers @{"Authorization"="Bearer MySecureAndUniqueToken0401"; "Content-Type"="application/json"} -Body "{}"
```

Expected Response:
```bash
{
  "source_text": "default text for processing",
  "token_count": 4,
  "is_question_present": false,
  "all_caps_tokens": [],
  "analysis_time": 1234567890.1234567
}
```
### Project Structure
```bash
.
├── .env
├── business_service.py
├── client_service.py
├── db_service.py
└── README.md
```
