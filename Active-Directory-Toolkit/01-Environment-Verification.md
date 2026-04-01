# 🛠️ Lab 01: Environment & Module Verification

**Scenario:** Before performing administrative tasks or security audits, a script must verify that the necessary PowerShell modules are present and that it can successfully communicate with the Domain Controller.

## 📝 Objective
Create a "sanity check" script that validates the presence of the `ActiveDirectory` module and pulls basic domain metadata.

## 💻 The Script
```powershell
# Check for AD Module availability
if (Get-Module -ListAvailable -Name ActiveDirectory) {
    Write-Host "[+] AD Module is available. Importing..." -ForegroundColor Green
    Import-Module ActiveDirectory
    
    # Retrieve basic domain info for verification
    $Domain = Get-ADDomain
    $UserCount = (Get-ADUser -Filter *).Count
    
    Write-Host "[+] Connected to: $($Domain.NetBIOSName)" -ForegroundColor Cyan
    Write-Host "[+] Total User Objects Found: $UserCount" -ForegroundColor Cyan
} else {
    Write-Error "[-] ActiveDirectory module not found. Please install RSAT."
}
