# File Integrity Monitor (FIM) Implementation using PowerShell

## 🎯 Objective
File Integrity Monitoring (FIM) is a critical security control used to detect unauthorized changes to files and directories. This project demonstrates the creation of a custom FIM tool using PowerShell. The script leverages the `.NET System.IO.FileSystemWatcher` class to monitor a designated directory in real-time, instantly alerting on file creation, modification, deletion, and renaming events. This type of monitoring is essential for detecting early signs of ransomware activity, malware persistence, or data exfiltration.

## 🛠️ The PowerShell Script (With Logging)
To ensure the script functions as a background agent, it was designed to write alerts to a permanent log file (`FIM_Log.txt`) in addition to standard console output. 

```powershell
# Define the folder we want to protect
$path = "C:\Users\thomas.price\Documents\SensitiveData"

# Create the folder if it doesn't exist yet
if (!(Test-Path $path)) { 
    New-Item -ItemType Directory -Path $path | Out-Null
    Write-Host "[+] Created directory: $path" -ForegroundColor Green
}

# Set up the File System Watcher
$watcher = New-Object System.IO.FileSystemWatcher
$watcher.Path = $path
$watcher.Filter = "*.*"
$watcher.IncludeSubdirectories = $true
$watcher.EnableRaisingEvents = $true

# Define what happens when an alert triggers
$action = {
    $name = $Event.SourceEventArgs.Name
    $changeType = $Event.SourceEventArgs.ChangeType
    $timeStamp = $Event.TimeGenerated
    
    # Format the alert message
    $alertMessage = "[!] ALERT: File $changeType - Name: $name at $timeStamp"
    
    # Print it to the screen AND save it to a permanent log file
    Write-Host $alertMessage -ForegroundColor Red
    Add-Content -Path "C:\Users\thomas.price\Documents\FIM_Log.txt" -Value $alertMessage
}

# Register the events we want to watch
Register-ObjectEvent $watcher "Created" -Action $action | Out-Null
Register-ObjectEvent $watcher "Changed" -Action $action | Out-Null
Register-ObjectEvent $watcher "Deleted" -Action $action | Out-Null
Register-ObjectEvent $watcher "Renamed" -Action $action | Out-Null

Write-Host "[*] FIM Active: Monitoring started on $path..." -ForegroundColor Cyan
Write-Host "[*] Press Ctrl+C to stop." -ForegroundColor Gray

# Keep the script running
while ($true) { Start-Sleep -Seconds 1 }
```

## ⚙️ Establishing Enterprise Persistence
A standard PowerShell script requires an active, open console window to run. To deploy this FIM as an enterprise-grade agent, it must run invisibly in the background.

To achieve this, the script was registered as a Scheduled Task. To bypass user-login requirements and ensure the agent starts immediately at boot, it was assigned to the built-in `SYSTEM` account with the highest execution privileges. This deployment was automated via PowerShell:

```powershell
# Define what the task will do
$Action = New-ScheduledTaskAction -Execute "powershell.exe" -Argument "-ExecutionPolicy Bypass -WindowStyle Hidden -File C:\Users\thomas.price\Documents\PowerShell\Scripts\FIM_Monitor.ps1"

# Define when it will happen (At boot)
$Trigger = New-ScheduledTaskTrigger -AtStartup

# Define WHO runs it (The SYSTEM account, with highest privileges)
$Principal = New-ScheduledTaskPrincipal -UserId "SYSTEM" -LogonType ServiceAccount -RunLevel Highest

# Build and register the task
Register-ScheduledTask -TaskName "CyberRange FIM Agent" -Action $Action -Trigger $Trigger -Principal $Principal

Write-Host "[+] FIM Agent successfully installed as a SYSTEM service." -ForegroundColor Green
```

## 🛡️ Management and Execution
Because the task is running under the `SYSTEM` context, standard GUI tools like the Windows Task Scheduler may obscure its visibility depending on execution contexts. The service was managed and verified entirely via the command line.

The following commands were used to verify the task registration and force the agent to start its monitoring process:

```powershell
# Verify the task exists in the registry
Get-ScheduledTask -TaskName "CyberRange FIM Agent"

# Force start the invisible background agent
Start-ScheduledTask -TaskName "CyberRange FIM Agent"
```

To validate the effectiveness of the File System Watcher, test files and directories were introduced to the monitored `SensitiveData` directory. The lifecycle of the files was manipulated to trigger various event types (Creation, Renaming, Modification, and Deletion). As shown in the management console below, the agent successfully detected and logged every stage of the file modifications.

![FIM Management](./fim_management.png)

## 🔍 Conclusion
This custom FIM tool successfully demonstrates the core mechanics behind enterprise Endpoint Detection and Response (EDR) agents. By establishing persistence as a `SYSTEM` service and capturing file system events in real-time, analysts can quietly monitor critical directories and generate permanent logs of unauthorized modifications before a threat actor can fully compromise the environment.
