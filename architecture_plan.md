# Architecture & Implementation Plan

## Service Mapping

| Layer | Service (Azure) | Role in Solution | Related Module |
| :--- | :--- | :--- | :--- |
| **Frontend/Compute** | Azure App Service | Hosts the Flask web application for patients and clinicians. | Module 3: Containers & Web Apps |
| **Object Storage** | Azure Blob Storage | Serves as the landing zone for raw JSON telemetry and patient roster CSVs. | Module 2: Cloud Storage |
| **Serverless Logic** | Azure Functions | Event-driven trigger that validates readings and calculates risk flags. | Module 5: Serverless |
| **Database** | Azure SQL Database | Stores structured, cleaned readings for historical analysis and reporting. | Module 4: Managed Databases |
| **Security** | Azure Key Vault | Securely manages database strings and API secrets. | Module 6: Security & Governance |

## Data Flow Narrative

1. Ingestion: A patient submits their daily blood pressure via a flask web form. The app sends this data as a JSON file to a "raw" container in azure blob storage.
2. Processing: The upload triggers an azure function. The function parses the JSON, checks for data integrity, and compares the readings against clinical thresholds.
3. Enrichment: The function assigns a risk level (Normal, Elevated, or Stage 1/2 Hypertension) based on the systolic/diastolic values.
4. Persistence: The cleaned data and the new risk flag are inserted into the azure SQL database.
5. Visualization: When a nurse logs into the dashboard, the Flask app queries the SQL database to pull a "Daily Summary" of all patients flagged as "High Risk" for immediate follow-up.

## Security and Governance

To ensure the security of this solution, all credentials and connection strings are stored in azure key vault. The flask application and azure functions use managed identities to authenticate with the database, meaning no passwords are stored in the source code. 

From a governance perspective, the system avoids storing protected health information (PHI) by using synthetic identifiers (UUIDs) for patients. The actual mapping of UUIDs to real patient names would remain within the clinic's internal, firewalled EHR system, keeping the cloud environment "de-identified." role-based access control (RBAC) is used to ensure the flask app has "contributor" access to storage, while the azure function has "writer" access to the SQL database.

## Cost and Operational Considerations

Cost Optimization: The azure SQL database is configured to the "serverless" compute tier, which automatically scales and pauses during late-night hours when no readings are being submitted. This fits well within a student budget.

Serverless Efficiency: Using azure functions instead of a dedicated virtual machine for data processing ensures that we only pay for the seconds the code is actually running.

Storage Lifecycle: Raw JSON files in Blob Storage are set to move to "cool" or "archive" storage after 30 days to further reduce costs.

## Architecture Diagram



```mermaid
graph LR
    User((Patient)) -->|Submit Reading| Flask[Azure App Service]
    Flask -->|Store JSON| Blob[(Azure Blob Storage)]
    Blob -->|Trigger| AF[Azure Function]
    AF -->|Assign Risk Flag| DB[(Azure SQL DB)]
    DB -->|Fetch Summary| Flask
    Flask -->|Display Dashboard| Staff((Clinic Staff))
