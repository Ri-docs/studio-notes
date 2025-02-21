# Configuring the OneRoster CSV Source
## 1. Introduction

This guide covers how to configure RapidIdentity Studio to import OneRoster CSV files from an SFTP server. It addresses creating a “source” application, handling connection settings, troubleshooting errors, and verifying data in Studio.

---

## 2. Creating and Configuring the OneRoster CSV Source

1. **Access the Studio Module**
    
    - From RapidIdentity, go to **Studio** > **Applications**.
2. **Add a New Application**
    
    - Click **Add Application**.
    - Select **From App Library** (recommended if you want a preconfigured OneRoster setup).
        - Alternatively, choose **Create New** if you need a custom workflow.
3. **Choose the OneRoster CSV Provider**
    
    - Filter by Source type and look for “**OneRoster CSV Provider**.”
    - Click **Install**, then open the three-dot menu to **Configure**.
4. **SFTP Connection Details**
    
    - In **Connection Settings**:
        - **Host**: Enter the IP address or hostname of the SFTP server.
        - **Port**: Enter the port number used by the SFTP server.
        - **Path**: Provide either:
            - A path to a **.zip** file containing all your OneRoster CSV files (recommended), or
            - A path to a **directory** that holds individual CSV files.
        - **User Directory is Root**: If files live at the top level of the user’s SFTP directory, enable this option.
        - **Credentials**: Enter the SFTP username and password (or private key if required).
5. **Enable Record Definitions**
    
    - Studio requires at least one active Record Definition to parse CSV data (e.g., **users**, **orgs**, **courses**).
    - In the **Schema** or **Advanced** tab of the application configuration, verify that relevant record types (e.g., “ORCSV-users,” “ORCSV-orgs”) are **enabled**.
6. **Save and Test**
    
    - Click **Test Connection** (if available) to ensure Studio can reach the SFTP server.
    - If the connection test fails, confirm credentials, file paths, and that the server is accessible on the specified port.
7. **Run the Job**
    
    - In **Studio** > **Jobs** > **All Jobs**, locate your new source application job.
    - Click **Run** to ingest the files.
    - Once complete, check **Data Explorer** for the new source data (e.g., “ORCSV-users” or “ORCSV-orgs”).

---

## 3. Common Issues & Troubleshooting

8. **SFTP Connection Fails**
    
    - **Symptom**: “Connection timed out” or “Connection closed” in your terminal.
    - **Solution**:
        - Confirm SFTP server is running and reachable (use `sftp` from a command line to test).
        - Double-check port settings and firewall rules.
        - Verify the username/password or key pair.
9. **Invalid Path Errors**
    
    - **Symptom**: Studio logs showing “Could not read from…” or “Not a file.”
    - **Solution**:
        - Ensure the **Path** matches exactly (e.g., `"/data/data.zip"`).
        - If uploading individual CSVs instead of a ZIP, confirm the directory structure and file names match what Studio expects.
        - For a ZIP file, verify the CSVs are indeed inside the archive and spelled correctly.
10. **At Least One Record Definition Must Be Enabled**
    
    - **Symptom**: A 400 error stating “At least one Record Definition must be present and enabled.”
    - **Solution**:
        - In the application’s **Schema** or **Record Definitions** section, enable at least one record type.
        - Studio must know how to parse each CSV file (e.g., “academicSession,” “orgs,” “users,” etc.).
11. **Zero Records Imported**
    
    - **Symptom**: Logs show the job running but result in “0 records imported.”
    - **Solution**:
        - Check that the CSV headers align with OneRoster or the custom field definitions you’ve mapped.
        - Verify the CSV files aren’t empty and that your date/time formats are correct (e.g., `YYYY-MM-DDThh:mm:ssZ` for timestamps).
12. **No Data in Data Explorer**
    
    - **Symptom**: Even after uploading files, you don’t see them in **Data Explorer**.
    - **Solution**:
        - Run the job manually or wait for the scheduled run to complete.
        - Confirm the job didn’t fail.
        - Look at the logs (in **Jobs** > **View Logs**) to see if it fully processed or flagged any errors.

---

## 4. Best Practices

13. **Using ZIP Files Instead of Individual CSVs**
    
    - Shortens the time Studio needs to maintain an open SFTP connection.
    - Minimizes risk if the connection drops mid-transfer.
    - Simplifies the ingestion process by grouping all relevant CSVs into a single atomic archive.
14. **Naming Conventions**
    
    - Keep CSV file names consistent with OneRoster standards (e.g., `orgs.csv`, `users.csv`).
    - Use clear labeling in Studio for your application and record definitions (e.g., “ORCSV-orgs,” “ORCSV-users”) to easily track them later.
15. **Check Logs in Detail Mode**
    
    - Running jobs in “trace” or “debug” mode can uncover parsing issues or missing data fields.
    - For each job, a detailed log shows whether the files were recognized, unzipped, and parsed.
16. **Regular Maintenance**
    
    - Periodically confirm that SFTP credentials, server IPs, and file structures haven’t changed.
    - If the SIS or data format changes, update the field mappings and record definitions accordingly.

---

## 5. Next Steps

- **Verification**:
    - Once records appear in **Data Explorer** for each entity (users, orgs, etc.), you can further validate or transform fields as needed.
- **Mapping to Targets**:
    - If you plan to send this data to other applications, configure Target apps and set up Access Groups to control which data is shared.
- **Further Exploration**:
    - Familiarize yourself with additional Studio features (e.g., scheduling multiple daily runs, using expressions to manipulate fields, merging extra data) to fully automate your rostering process.
