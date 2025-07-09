# Automating DB with Powershell & Python:-

## 1. PowerShell Script: Rebuild Indexes Across All Databases with Logging
```
$ServerInstance = "YourServerName"
$LogFile = "C:\Logs\IndexRebuildLog.txt"
$Databases = Invoke-Sqlcmd -ServerInstance $ServerInstance -Query "SELECT name FROM sys.databases WHERE database_id > 4"

foreach ($db in $Databases) {
    $dbName = $db.name
    Add-Content $LogFile "Starting index rebuild for database: $dbName at $(Get-Date)"
    try {
        Invoke-Sqlcmd -ServerInstance $ServerInstance -Database $dbName -Query "EXEC sp_MSforeachtable 'ALTER INDEX ALL ON ? REBUILD'"
        Add-Content $LogFile "Success: Indexes rebuilt for $dbName at $(Get-Date)"
    } catch {
        Add-Content $LogFile "Error: $_ for $dbName at $(Get-Date)"
    }
}

```

## 2. Python Script: Load Excel Files into SQL Server with pandas:-

```
import os
import pandas as pd
import pyodbc

excel_folder = "C:/Data/ExcelFiles"
conn_str = "DRIVER={ODBC Driver 17 for SQL Server};SERVER=your_server;DATABASE=your_db;Trusted_Connection=yes"
conn = pyodbc.connect(conn_str)
cursor = conn.cursor()

def clean_column(col):
    return col.strip().replace(" ", "_")

for file in os.listdir(excel_folder):
    if file.endswith(".xlsx"):
        path = os.path.join(excel_folder, file)
        df = pd.read_excel(path)
        df.columns = [clean_column(col) for col in df.columns]
        for _, row in df.iterrows():
            cursor.execute("INSERT INTO YourTable (Col1, Col2) VALUES (?, ?)", row.Col1, row.Col2)
        conn.commit()
```

## 3. PowerShell Script: Run SQL Files for DevOps Deployment:-
```
$ServerInstance = "YourServerName"
$Database = "YourDatabase"
$ScriptFolder = "C:\Deployment\Scripts"
$LogFile = "C:\Logs\DeploymentLog.txt"

Get-ChildItem -Path $ScriptFolder -Filter "*.sql" | Sort-Object Name | ForEach-Object {
    $scriptPath = $_.FullName
    Add-Content $LogFile "Running script: $scriptPath at $(Get-Date)"
    try {
        Invoke-Sqlcmd -ServerInstance $ServerInstance -Database $Database -InputFile $scriptPath
        Add-Content $LogFile "Success: $scriptPath at $(Get-Date)"
    } catch {
        Add-Content $LogFile "Error: $_ while running $scriptPath at $(Get-Date)"
    }
}
```

## 4. Python Script: Check Failed SQL Agent Jobs and Send Email:-

```
import smtplib
from email.mime.text import MIMEText
import pyodbc

conn = pyodbc.connect("DRIVER={ODBC Driver 17 for SQL Server};SERVER=your_server;DATABASE=msdb;Trusted_Connection=yes")
cursor = conn.cursor()

cursor.execute("""
SELECT name, last_run_date, last_run_time, last_run_outcome
FROM sysjobs j
JOIN sysjobservers s ON j.job_id = s.job_id
WHERE last_run_outcome != 1
""")

failed_jobs = cursor.fetchall()

if failed_jobs:
    message = "Failed SQL Agent Jobs:\n\n"
    for job in failed_jobs:
        message += f"Job: {job.name}, Last Run Date: {job.last_run_date}, Time: {job.last_run_time}\n"

    msg = MIMEText(message)
    msg['Subject'] = 'SQL Agent Job Failures'
    msg['From'] = 'noreply@yourdomain.com'
    msg['To'] = 'dba_team@yourdomain.com'

    with smtplib.SMTP('smtp.yourdomain.com') as server:
        server.send_message(msg)
```
