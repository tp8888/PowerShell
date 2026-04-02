# 🛡️ Lab 03: Privileged Group Membership Audit

**Scenario:** As part of a routine security assessment, the SOC needs to verify exactly who holds the "keys to the kingdom." Threat actors frequently attempt to add compromised accounts to privileged groups to maintain persistence. We need a script to automatically audit these high-risk groups, handle potential errors gracefully, and expose any unauthorized users.

## 📝 Objective
Create a robust PowerShell script that iterates through highly privileged Active Directory groups (e.g., Domain Admins, Enterprise Admins) and exports a clean report of all current members, identifying any accounts that should not be present.

## 💻 The Final Audit Script
The following script was executed from the **Windows 10 Enterprise** jump-box to audit the `ad.lab` domain.

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

---

## 🛠️ Command Reference (Troubleshooting & Setup)
During the lab, several supplemental commands were used to verify the environment and simulate a compromised state.

### 1. User Verification
Used to find the exact `SamAccountName` of users created by the automated lab script.
```powershell
Get-ADUser -Filter * | Select-Object Name, SamAccountName
```

### 2. Manual Privilege Escalation (Attacker Simulation)
Executed on the **Domain Controller** to simulate an attacker adding compromised accounts to high-privilege groups.
```powershell
# Add user to Domain Admins
Add-ADGroupMember -Identity "Domain Admins" -Members "melisent.karola"

# Add user to Enterprise Admins
Add-ADGroupMember -Identity "Enterprise Admins" -Members "marya.colette"
```

### 3. Identity Resolution via Pipeline
A "fail-safe" method used when standard identity strings failed to resolve. This pipes the group object directly into the membership command.
```powershell
Get-ADGroup -Filter {Name -eq 'Domain Admins'} | Add-ADGroupMember -Members "melisent.karola"
```

### 4. Module Troubleshooting
Used to ensure the Active Directory tools were available in the current PowerShell session.
```powershell
Import-Module ActiveDirectory
```

---

## 🔍 Code Breakdown
* **`try { ... } catch { ... }`**: Prevents script termination if a group is missing or permissions are insufficient.
* **`[PSCustomObject]`**: Filters out unnecessary AD metadata, leaving only actionable data for a report.
* **`$AuditResults | Format-Table -AutoSize`**: Formats the collected data into a readable table, automatically adjusting column widths.

## 🧪 Lab Results
* **Script Execution:** Success. The script handled the missing "Schema Admins" group gracefully.
* **Unauthorized Access Detected:** The audit successfully identified two non-administrative accounts that had been elevated to privileged groups:
  * `melisent.karola` discovered in **Domain Admins**.
  * `marya.colette` discovered in **Enterprise Admins**.
