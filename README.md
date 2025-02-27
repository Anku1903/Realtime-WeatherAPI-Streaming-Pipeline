# azure-realtime-weather-pipeline
An end-to-end, real-time weather analytics pipeline on Azure and Microsoft Fabric

### Description
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
