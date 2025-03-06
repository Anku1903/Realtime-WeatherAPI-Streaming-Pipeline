# azure-realtime-weather-pipeline
An end-to-end, real-time weather analytics pipeline on Azure and Microsoft Fabric

## Overview
This project demonstrates an end-to-end real-time data engineering solution using Azure services and Microsoft Fabric to continuously ingest, process, store, and visualize weather data. It begins by pulling data from a free Weather API, then uses Azure Databricks and Azure Functions for data ingestion into Azure Event Hub. From there, Microsoft Fabric’s real-time intelligence features (Event Stream and Kusto DB) manage the streaming and storage, keeping the Power BI dashboard continuously up-to-date. The project also covers secure credential handling with Azure Key Vault.

## Technologies Used
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

- Configuring the Weather API: Signed up for the Weather API, verified the account, and retrieved the API key.
- Creating a Resource Group: Set up a dedicated resource group to organize all project components.
- Deploying Core Azure Services:
  - Azure Databricks Workspace: Created for data processing.
  - Azure Functions App: Set up for serverless data ingestion.
  - Azure Event Hub: Provisioned to manage streaming data.
  - Azure Key Vault: Established to securely store sensitive information, including the Weather API key.

 ### Stage 2 - Data Ingestion
 In this stage I explored 2 different approaches for data ingestion - Databricks and Azure Functions.

 #### Azure Databricks
 The data ingestion pipeline was built and tested incrementally. The the steps included:
 1. Intiallized a cluster
 2. Installed **azure-eventhub** on the cluster - an external Python library, not originally provided by Databricks, to enable interation with the Event Hub
 3. Tested the connection between **Databricks** and **Event Hub** by sending simple JSON test events
      - monitored the received test events by using the Data Explorer feature in Event Hub
      - for security purposes, replaced the plain text Event Hub Connection String. To achieve this: 
        - created **Databricks Secret Scope** - to refer and access secrets from the **Azure Key Vault**
        - via Azure Key Vault assigned Databricks the role **"Key Vault Secret User"** (i.e. gave Databricks read permission and fixed the innitial "Permission denied" error)
        - plain text connection string was replaced by function to extract it from the Key Vault 
           ```python
          eventhub_connection_string = dbutils.secrets.get(scope="key-vault-scope", key="eventhub-connection-string")
          ```
        ![databricks_send_test_events_to_eventhub](https://github.com/user-attachments/assets/8f7ec1fa-7a74-496d-940f-8e7838202987)
    
   
   4. Tested the connection between **Databricks** and **WeatherAPI**
      - Got the secret API key though the Databricks Secret Scope
      - Sat up the required parameters by the Weather API documentation
      - Printed the API response directly in the notebook 
        ![databricks_test_weateherAPI](https://github.com/user-attachments/assets/6578245d-22f3-4faa-9928-0d7d9f80c9ef)
   
   
   5. Connect all WeatherAPI, Databricks and Event Hub  
      In the final step, the pipeline connects all three components as follows:
      - Weather API integration
        - A comprehensive script in Azure Databricks retrieves weather data (current conditions, forecasts, alerts) from the Weather API
        - The API responses are processed and flattened into a structured JSON format
      - Databricks Streaming Setup
        Simulated continuous ingestion process by using Spark Structured Streaming since the Weather API itself isn’t a native streaming source
        - Created a dummy stream that serves as a trigger to periodically execute the processing logic
          ```python
          streaming_df = spark.readStream.format("rate").option("rowsPerSecond", 1).load()
          ```
        - Used Micro-Batch Processing with foreachBatch  
          Spark Structured Streaming processes incoming data in micro-batches where each batch is processed by custom logic
          ```python
          query = streaming_df.writeStream.foreachBatch(process_batch).start()
          ```
        - Timer mechanism to fetch data every 30 seconds  
          Despite a micro-batch is triggered every second, continuously querying the Weather API every second is inefficient and unnecessary. To manage this, the **process_batch** function includes a timer check.
          ```python
          def process_batch(batch_df, batch_id):
            global last_sent_time
            try:
                # Get current time
                current_time = datetime.now()
                
                # Check if X seconds have passed since last event was sent
                if (current_time - last_sent_time).total_seconds() >= 30:
                  # Fetch weather data
                  weather_data = fetch_weather_data()
          
                  # Send the weather data 
                  send_event(weather_data)
          
                  # Update last sent time
                  last_sent_time = current_time
                  print(f"Event sent at {last_sent_time}")
          
            except Exception as e:
                print(f"Error sending events {batch_id}: {e}")
                raise e 
          ```
 
 ### Azure Function App
 The key steps in the development in this alternative approach include:
 1. Setting up VS Code to build the data ingestion function on my local machine
    - Installed the **Azure Functions extension** to enable creating and managing function projects
    - Linked local development with cloud resources by signing in to my Azure subscription from VS Code
 
 2. Creating the Function Project  
    - Selected Python and **Time Trigger with a CRON expression** to run every 30 sec
    - Let VS code generate the starter template
 
 3. Adapting the data ingestion code from Databricks
    - Removed the Spark-specific components (e.g The Spark Structured Streaming)
    - Replaced the Databricks secret scope access with **Managed Identity** and assigned appropriate roles
        -  **"Azure Event Hub Data Sender"** role for Event Hub acess
        -  **"Key Vault Secret User"** role for the Key Vault

 4. Deployment and testing
    - Used the Azure Function extention for VS Code to deploy the function to Azure 
    - Verified the function is correctly deployed and events are successfully sent to the Event Hub every 30 seconds
      ![function_app_success_deployed](https://github.com/user-attachments/assets/d39f54c6-2b54-435c-9c96-1aa393166430)

    
         





