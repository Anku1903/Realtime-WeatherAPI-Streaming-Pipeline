# Realtime-WeatherAPI-Streaming-Pipeline

## Overview
This repository contains an Azure-based data engineering pipeline designed to fetch real-time weather data from a public Weather API, process it, store it, and create interactive dashboards for visualization. Additionally, the pipeline includes an alerting mechanism that sends email notifications when critical weather conditions (e.g., extreme temperatures, high winds) are detected. Built using Microsoft Azure services, the project demonstrates a scalable, secure, and efficient architecture for real-time weather data processing and analytics.

The pipeline leverages Azure Event Hubs for data streaming, Azure Databricks for processing, Azure Data Lake Storage Gen2 for storage, Azure Functions for serverless alerting, Azure Key Vault for secure secret management, and Microsoft Fabric for unified analytics and dashboard creation. This README provides a comprehensive guide to the project's structure, technologies, setup instructions, and usage.

## Table of Contents
- [Technologies Used](#technologies-used)
- [Project Architecture](#project-architecture)
- [Detailed Explanation](#detailed-explanation)

## Technologies Used
The following table outlines the technologies and tools used in this project:

| Technology/Tool               | Purpose                                                                 |
|-------------------------------|-------------------------------------------------------------------------|
| Azure Event Hubs              | Real-time data streaming and ingestion of weather data                  |
| Azure Databricks              | Distributed data processing and transformation of weather data           |
| Azure Functions               | Serverless functions for triggering email alerts on critical conditions  |
| Azure Data Lake Storage Gen2  | Scalable storage for raw and processed weather data                      |
| Azure Key Vault               | Secure management of API keys, credentials, and secrets                 |
| Microsoft Fabric              | Unified analytics platform for data integration and dashboard creation   |
| Python                        | Core programming language for scripting and data processing              |
| Weather API                   | Source of real-time weather data (e.g., OpenWeatherMap)                 |

## Project Architecture
The pipeline follows a modular and scalable architecture built on Azure services. Below is a high-level overview of the data flow:

1. **Data Ingestion**: A Python script fetches real-time weather data from a Weather API and sends it to Azure Event Hubs.
2. **Data Streaming**: Azure Event Hubs streams the data to downstream processing components.
3. **Data Processing**: Azure Databricks processes the streamed data, performing transformations and detecting critical conditions.
4. **Data Storage**: Processed data is stored in Azure Data Lake Storage Gen2 for analytics and archival.
5. **Alerting**: Azure Functions triggers email alerts when critical weather conditions are detected.
6. **Secret Management**: Azure Key Vault securely stores API keys and credentials.
7. **Visualization**: Microsoft Fabric creates interactive dashboards to visualize weather trends and metrics.

## Detailed Explanation
### 1. Data Ingestion
- **Source**: Real-time weather data (e.g., temperature, humidity, wind speed) is fetched from a Weather API using Python's `requests` library.
- **Frequency**: Data is fetched at regular intervals (e.g., every 5 minutes).
- **Output**: Raw JSON data is sent to Azure Event Hubs for streaming.

### 2. Data Streaming with Azure Event Hubs
- Azure Event Hubs serves as the real-time data streaming platform, ingesting weather data with low latency and high throughput.
- A Python producer script publishes weather data to an Event Hubs namespace.
- Consumers (e.g., Azure Databricks) subscribe to the event stream for processing.

### 3. Data Processing with Azure Databricks
- Azure Databricks, running Apache Spark, consumes data from Event Hubs.
- Processing tasks include:
  - Cleaning and validating raw data.
  - Aggregating metrics (e.g., average temperature by region).
  - Detecting critical weather conditions (e.g., temperature > 40°C or wind speed > 50 km/h).
- Processed data is written to Azure Data Lake Storage Gen2 in Parquet format for efficient storage and querying.

### 4. Data Storage with Azure Data Lake Storage Gen2
- Azure Data Lake Storage Gen2 stores both raw and processed weather data.
- Data is organized in a hierarchical structure (e.g., `/raw/year/month/day/` and `/processed/year/month/day/`).
- The storage solution supports scalability and integration with analytics tools like Microsoft Fabric.

### 5. Alerting with Azure Functions
- Azure Functions monitors processed data for critical weather conditions.
- When thresholds are exceeded (e.g., extreme heat or storms), a serverless function triggers an email alert using an SMTP service (e.g., SendGrid or Microsoft 365).
- Functions are lightweight and cost-efficient, ensuring rapid response to critical events.

### 6. Secret Management with Azure Key Vault
- Azure Key Vault securely stores sensitive information, such as:
  - Weather API keys.
  - SMTP credentials for email alerts.
  - Connection strings for Event Hubs and Data Lake.
- Databricks and Functions retrieve secrets at runtime, ensuring security and compliance.

### 7. Visualization with Microsoft Fabric
- Microsoft Fabric integrates data from Azure Data Lake Storage Gen2 for analytics.
- Interactive dashboards are created to visualize:
  - Real-time weather metrics (e.g., temperature, humidity trends).
  - Historical weather patterns.
  - Alerts for critical conditions.
- Fabric's unified platform simplifies data exploration and reporting.
