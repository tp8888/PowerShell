# 🏢 Lab 02: Automated Bulk User Creation

**Scenario:** An organization is onboarding a new department. To ensure consistency and save time, we will automate the creation of these user accounts using a CSV template and the `New-ADUser` cmdlet.

## 📝 Objective
Write a script that reads user data from a CSV file and creates Active Directory accounts with standardized attributes (Department, Title, and Password).

## 📊 The Data Template (users.csv)
Before running the script, create a file named `users.csv` in your lab directory with the following headers:
`firstname,lastname,username,department,title`

## 💻 The Script
```powershell
# Define the path to the CSV file
$CSVPath = "C:\LabFiles\users.csv"

# Import the data
$Users = Import-Csv -Path $CSVPath

foreach ($User in $Users) {
    # Construct the Distinguished Name (Target OU)
    $TargetOU = "OU=Employees,DC=Lab,DC=Local"
    
    # Create the account
    New-ADUser -Name "$($User.firstname) $($User.lastname)" `
               -GivenName $User.firstname `
               -Surname $User.lastname `
               -SamAccountName $User.username `
               -UserPrincipalName "$($User.username)@lab.local" `
               -Path $TargetOU `
               -Department $User.department `
               -Title $User.title `
               -Enabled $true `
               -ChangePasswordAtLogon $true
               
    Write-Host "[+] Successfully created account for: $($User.username)" -ForegroundColor Green
}
