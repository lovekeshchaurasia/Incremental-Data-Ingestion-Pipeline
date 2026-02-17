# Incremental-Data-Ingestion-Pipeline
This project demonstrates an incremental data ingestion pipeline built using Azure Data Factory (ADF) and Azure SQL Database. The pipeline is designed to load only new or updated records from a source file based on a timestamp watermark mechanism, avoiding full reloads and enabling efficient ETL processing.

The ingestion process uses a JSON file to store the last processed timestamp and checks for new records before performing the data copy operation.

📌 Key Features

Timestamp-based incremental load implementation

Watermark stored in a JSON configuration file

Conditional execution using If-Else activity

Loads only records with timestamp greater than the last processed value

Updates watermark after successful ingestion

Data stored in Azure SQL Database

Output files stored in Azure Data Lake / Blob Storage

⚙️ Pipeline Workflow
1. Watermark Storage

A JSON file contains the last processed timestamp:

{
  "last_timestamp": "2026-02-14T10:00:00"
}

2. Check for New Records

The pipeline first runs a SQL/script query to count records where:

**updated_timestamp > last_timestamp**

3. Conditional Execution

If record_count > 0 → new data exists → pipeline proceeds

If record_count = 0 → no new data → pipeline stops

4. Copy New Records

Only new records are ingested into the target Azure SQL table using Copy Activity / Script Activity.

5. Update the Watermark

After ingestion, the pipeline selects the maximum timestamp from the newly loaded records:

SELECT MAX(updated_timestamp)
FROM Orders


This value is written back into the JSON file to ensure the next run continues incrementally.

🛠️ Technologies Used

Azure Data Factory

Azure SQL Database

Azure Blob Storage / Data Lake

JSON-based Watermarking

SQL Queries & Script Activities

🚀 Use Case

This pipeline is useful for real-time or scheduled ETL workflows where only incremental data needs to be ingested, such as:

Order processing systems

Customer transaction updates

Log/event ingestion

Data warehouse incremental refresh

📷 Screenshots

Screenshots included in this repository show:

Sink container storage output

Record count check activity

If-Else condition execution

Delete confirmation and incremental file updates

✅ Outcome
<img width="1170" height="787" alt="Screenshot 2026-02-15 141458" src="https://github.com/user-attachments/assets/7f22a69d-9b81-4476-9d2f-d72e77ae285b" />


This project ensures efficient incremental ingestion by:

Reducing unnecessary full loads

Maintaining state using timestamp watermark

Supporting scalable production-ready ETL patterns

