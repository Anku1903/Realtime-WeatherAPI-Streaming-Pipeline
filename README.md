# azure-realtime-weather-pipeline
An end-to-end, real-time weather analytics pipeline on Azure and Microsoft Fabric

# Description
This project demonstrates an end-to-end real-time data engineering solution using Azure services and Microsoft Fabric to continuously ingest, process, store, and visualize weather data. It begins by pulling data from a free Weather API, then uses Azure Databricks and Azure Functions for data ingestion into Azure Event Hub. From there, Microsoft Fabric’s real-time intelligence features—Event Stream and Kusto DB—handle streaming and storage, enabling an up-to-date Power BI dashboard. Additionally, Data Activator sends automated email alerts for extreme weather conditions. The project also covers architectural decisions, cost management, and secure credential handling with Azure Key Vault, making it an excellent hands-on introduction to modern real-time data pipelines.
