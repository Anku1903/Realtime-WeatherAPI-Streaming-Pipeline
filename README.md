# azure-realtime-weather-pipeline
An end-to-end, real-time weather analytics pipeline on Azure and Microsoft Fabric

### Overview
This project demonstrates an end-to-end real-time data engineering solution using Azure services and Microsoft Fabric to continuously ingest, process, store, and visualize weather data. It begins by pulling data from a free Weather API, then uses Azure Databricks and Azure Functions for data ingestion into Azure Event Hub. From there, Microsoft Fabric’s real-time intelligence features (Event Stream and Kusto DB) manage the streaming and storage, keeping the Power BI dashboard continuously up-to-date. The project also covers secure credential handling with Azure Key Vault.

### Technologies Used

- Azure
  - Azure Databricks
  - Azure Functions
  - Azure Event Hub
  - Azure Key Vault
- Microsoft Fabric
  - Event Stream
  - Kusto DB
- Power BI

## Development

### Stage 1
In this first stage, the environment was fully set up to support the real-time weather data pipeline. The key steps included:

Configuring the Weather API: Signed up for the Weather API, verified the account, and retrieved the API key.
Creating a Resource Group: Set up a dedicated resource group to organize all project components.
Deploying Core Azure Services:
- Azure Databricks Workspace: Created for data processing.
- Azure Functions App: Set up for serverless data ingestion.
- Azure Event Hub: Provisioned to manage streaming data.
- Azure Key Vault: Established to securely store sensitive information, including the Weather API key.
