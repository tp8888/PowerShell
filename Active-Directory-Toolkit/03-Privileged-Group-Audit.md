# 🛡️ Lab 03: Privileged Group Membership Audit

**Scenario:** As part of a routine security assessment, the SOC needs to verify exactly who holds the "keys to the kingdom." Threat actors frequently attempt to add compromised accounts to privileged groups to maintain persistence. We need a script to automatically audit these high-risk groups, handle potential errors gracefully, and expose any unauthorized users.

## 📝 Objective
Create a robust PowerShell script that iterates through highly privileged Active Directory groups (e.g., Domain Admins, Enterprise Admins) and exports a clean report of all current members, identifying any accounts that should not be present.

## 💻 The Script

The script utilizes a `Try/Catch` block to ensure that if a specific group (like Schema Admins) does not exist or is inaccessible in the current domain partition, the script issues a warning but continues executing rather than failing.

```powershell
# Define the high-risk groups we want to audit
$HighRiskGroups = @("Domain Admins", "Enterprise Admins", "Schema Admins")

# Create an empty array to store our findings
$AuditResults = @()

Write-Host "[*] Starting Privileged Group Audit..." -ForegroundColor Cyan

foreach ($Group in $HighRiskGroups) {
    Write-Host "    Scanning group: $Group" -ForegroundColor DarkGray
    
    try {
        # Added -ErrorAction Stop so if it fails, it jumps to the 'catch' block
        $Members = Get-ADGroupMember -Identity $Group -Server "ad.lab" -ErrorAction Stop
        
        foreach ($Member in $Members) {
            # Create a custom object for clean reporting
            $AuditResults += [PSCustomObject]@{
                GroupName   = $Group
                MemberName  = $Member.Name
                AccountName = $Member.SamAccountName
                ObjectType  = $Member.ObjectClass
            }
        }
    } catch {
        # If the group doesn't exist, print a warning but KEEP GOING
        Write-Host "    [!] Warning: Group '$Group' not found or inaccessible." -ForegroundColor Yellow
    }
}

# Display the results in the console as a clean table
Write-Host "`n[+] Audit Complete. Privileged Users Found:" -ForegroundColor Green
$AuditResults | Format-Table -AutoSize
```

## 🔍 Code Breakdown
* **`try { ... } catch { ... }`**: Prevents script termination if a group is missing or permissions are insufficient for a specific container.
* **`[PSCustomObject]`**: Filters out unnecessary Active Directory metadata, leaving only the actionable data needed for an incident response report.
* **`-Server "ad.lab"`**: Ensures the query targets the specific lab Domain Controller from the Windows 10 Enterprise jump-box.

## 🧪 Lab Results
* **Script Execution:** Success. The script handled the missing "Schema Admins" group gracefully using the defined error handling.
* **Unauthorized Access Detected:** The audit successfully identified two non-administrative accounts that had been elevated to privileged groups:
  * `melisent.karola` discovered in **Domain Admins**.
  * `marya.colette` discovered in **Enterprise Admins**.
