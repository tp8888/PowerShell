# ⚠️ Lab 00: Making Active Directory Exploitable (Vulnerable AD Plus)

**Scenario:** To properly simulate attacks and test security monitoring tools (like Suricata and Sentinel), the Active Directory environment must be intentionally misconfigured. This documentation outlines the foundational steps taken to introduce vulnerabilities and deploy exploitable user accounts into the `ad.lab` domain before any defensive auditing or PowerShell administration takes place.

## 📝 Objective
Deploy the "Vulnerable AD Plus" PowerShell script to populate the directory with vulnerable users and modify Group Policy Objects (GPOs) to disable built-in Windows protections.

## 💻 1. Executing the Vulnerable AD Script

To begin the process, Windows PowerShell was opened on the Domain Controller with Administrator privileges.

First, the system's execution policy was bypassed to allow the external script to run:

```powershell
Set-ExecutionPolicy -ExecutionPolicy Bypass -Force
```

Next, the script was downloaded and executed directly in memory. The command utilizes `-replace` to change the default `'change\.me'` string to our specific domain name (`ad.lab`) prior to execution:

```powershell
[System.Net.WebClient]::new().DownloadString('[https://raw.githubusercontent.com/WaterExecution/vulnerable-AD-plus/master/vulnadplus.ps1](https://raw.githubusercontent.com/WaterExecution/vulnerable-AD-plus/master/vulnadplus.ps1)') -replace 'change\.me', 'ad.lab' | Invoke-Expression
```

### 👤 Vulnerable Users Created
Upon execution, the script automatically generated several new users and groups with intentionally weak security postures (e.g., AS-REP Roastable accounts, Kerberoastable accounts, and passwords stored in descriptions). Based on the terminal output, the following vulnerable users were added to the environment:
* `fenelia.anette`
* `erina.larina`
* `milicent.judy`

*Note: Once the script completed, the system waited 30 seconds and automatically restarted.*

---

## 🛠️ 2. Group Policy Modifications

To ensure the lab remains vulnerable to network and remote attacks, several Group Policies were manually modified to simulate a poorly secured legacy environment.

### A. Disable Protections GPO
A new GPO named `Disable Protections` was created and enforced at the domain level to strip away default Windows defenses:
* **Windows Defender Antivirus:** Set to `Enabled` (which turns the antivirus off).
* **Real-Time Protection:** Set to `Enabled` (which turns real-time protection off).
* **Windows Defender Firewall (Domain Profile):** The "Protect all network connections" setting was configured to `Disabled`.

### B. Local Admin Remote Login GPO
To allow remote login capabilities for local administrators and facilitate lateral movement testing, a new GPO named `Local Admin Remote Login` was created:
* **Registry Path:** `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System`.
* **Registry Key:** A new `REG_DWORD` value named `LocalAccountTokenFilterPolicy` was created and set to `1`.

### C. Enable WinRM Server GPO
To facilitate remote management exploitation and unauthorized PowerShell remoting, the `Enable WinRM Server` GPO was deployed:
* **WinRM Remote Server Management:** Enabled with the IPv4 filter set to `*` to allow all IP addresses.
* **Basic Authentication:** Enabled.
* **Unencrypted Traffic:** Enabled.
* **WinRM Service:** Configured to automatically start the `Windows Remote Management (WS-Management)` service.
* **Remote Shell Access:** Enabled.

### D. Enable RDP & RPC GPOs
Finally, two separate GPOs were created to open up Remote Desktop Protocol and Remote Procedure Calls across the domain:
* **Enable RDP:** Configured to "Allow users to connect remotely using Remote Desktop Services".
* **Enable RPC:** Configured to "Enable RPC Endpoint Mapper Client Authentication".

---

## ⚡ 3. Enforcing the Changes

To ensure all newly created GPOs immediately propagated to all devices currently joined (or joining) the Active Directory environment, the following command was executed in an elevated PowerShell terminal:

```powershell
gpupdate /force
```

## 🧪 Lab Results
* **Vulnerable AD Script Execution:** Success
* **Domain Protections Disabled:** Yes
* **Environment Status:** Vulnerable & Ready for Attack Simulations
