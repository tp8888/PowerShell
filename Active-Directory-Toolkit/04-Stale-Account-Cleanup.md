# 🛡️ Lab 04: Stale Account Identification & Disablement

**Scenario:** To reduce the attack surface of the `ad.lab` domain, the SOC must identify and disable inactive user accounts. However, automation must be tempered with safety controls (Whitelisting) to ensure critical administrative or service accounts are never accidentally disabled, which could lead to a Domain-wide Denial of Service (DoS).

(../images/allaccounts diabled.png)


## 📝 Objective
Develop a PowerShell automation that identifies enabled user accounts based on a 30-day inactivity threshold and programmatically disables them while strictly excluding protected identities.

## 💻 The Final Hardened Script
This version includes the `$ExclusionList` (Whitelist) and `continue` logic to prevent accidental lockout of critical accounts.

```powershell
# 1. Define the stale threshold (e.g., 30 days)
$StaleDate = (Get-Date).AddDays(-30)

# 2. Define Protected Accounts (The Whitelist)
$ExclusionList = @("Administrator", "thomas.price", "krbtgt", "Guest")

Write-Host "[*] Searching for accounts inactive since $StaleDate..." -ForegroundColor Cyan

# 3. Retrieve currently enabled users
$UsersToDisable = Get-ADUser -Filter {Enabled -eq $true} -Properties LastLogonDate

if ($null -eq $UsersToDisable) {
    Write-Host "[!] No stale accounts found." -ForegroundColor Yellow
} else {
    $FinalAudit = foreach ($U in $UsersToDisable) {
        
        # Check if the user is in the Exclusion List
        if ($U.SamAccountName -in $ExclusionList) {
            Write-Host "    [!] Skipping Protected Account: $($U.SamAccountName)" -ForegroundColor Yellow
            continue # Move to the next user without disabling
        }

        # Check if the account is actually stale
        if ($U.LastLogonDate -lt $StaleDate -or $null -eq $U.LastLogonDate) {
            Write-Host "    [+] Disabling Stale Account: $($U.SamAccountName)" -ForegroundColor Gray
            Set-ADUser -Identity $U.SamAccountName -Enabled $false
            
            [PSCustomObject]@{
                User           = $U.Name
                SamAccountName = $U.SamAccountName
                LastLogon      = $U.LastLogonDate
                Status         = "DISABLED"
            }
        }
    }
    
    # 4. Display the final audit results
    $FinalAudit | Format-Table -AutoSize
}
```

---

## 🛠️ Command Reference (Recovery & Safety)
The following commands were used to recover the environment after the initial "Lockout Incident" and to configure the system for script execution.

### 1. Emergency Account Recovery
Executed from an authenticated jump-box session to restore access to disabled administrative accounts.
```powershell
Enable-ADAccount -Identity "Administrator" -Server "ad.lab"
Enable-ADAccount -Identity "thomas.price" -Server "ad.lab"
```

### 2. Adjusting Execution Policy
Required to allow the execution of local `.ps1` scripts on the Windows 10 workstation.
```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Force
```

### 3. Verification of Account Status
Used to confirm the `Enabled` status of a specific user object in Active Directory.
```powershell
Get-ADUser -Identity "Administrator" -Properties Enabled | Select-Object Name, Enabled
```

---

## 🔍 Code Breakdown
* **`$ExclusionList`**: A hard-coded array of usernames that the script is forbidden from modifying. This acts as a manual safety gate.
* **`continue`**: A flow-control keyword that skips the remaining code in the loop for the current object and immediately jumps to the next user.
* **`LastLogonDate -lt $StaleDate`**: Uses the "Less Than" operator to identify timestamps older than the defined 30-day window.

## 🧪 Lab Results
* **Incident Observed:** Initial script execution without an exclusion list resulted in a domain-wide lockout of the `Administrator` and `thomas.price` accounts.
* **Recovery:** Access was restored via the Windows 10 jump-box using the `Enable-ADAccount` cmdlet targeting the Domain Controller.
* **Remediation:** The script was updated with a whitelist. Subsequent testing (as seen in the console) confirmed the script successfully "skipped" protected accounts while still processing others.
