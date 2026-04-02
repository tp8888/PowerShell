# 💻 PowerShell Labs & Upskilling

Welcome to my central repository for PowerShell automation and security research. This project documents my professional growth in **Cybersecurity**, focusing on leveraging PowerShell for system administration, threat hunting, and infrastructure management.

## 🎯 Project Goals
The objective of this repository is to build a library of reusable scripts and detailed "Threat-Brief" style documentation that demonstrates proficiency in:
* **Enterprise Administration:** Managing Active Directory objects and Group Policy at scale.
* **Security Automation:** Parsing logs and identifying Indicators of Compromise (IoCs).
* **System Hardening:** Automating security configurations based on industry benchmarks.

---

## 🏗️ Lab Architecture: Multi-VM Enterprise Environment
To simulate a true corporate network, this lab utilizes a multi-VM architecture hosted in Oracle VirtualBox. This allows for testing remote administration and network-based security controls.

* **Domain Controller:** Windows Server 2019 (`ad.lab`)
* **Management Workstation:** Windows Enterprise (Version 22H2)
* **Connectivity:** Isolated via VirtualBox Internal Network to ensure a safe testing perimeter.
* **Tools:** Remote Server Administration Tools (RSAT) installed on the workstation to manage the DC via PowerShell.

---

## 🛠️ The "Golden Image" Workflow
I utilize a strict snapshot strategy to maintain environment integrity:
1. **Base Configuration:** Clean OS installs with all necessary modules pre-loaded.
2. **Snapshots:** A "Golden Image" snapshot is taken before any script execution.
3. **Rollback:** After testing high-impact or destructive scripts (like bulk user management or GPO changes), the workstation VM is rolled back to a clean state.

---

## 📂 Active Directory Toolkit: 10-Lab Roadmap

0. **[Lab 00: Vulnerable AD Configuration](./Active-Directory-Toolkit/00-Vulnerable-AD-Configuration.md)** ✅
1. **[Lab 01: Environment & Module Verification](./Active-Directory-Toolkit/01-Environment-Verification.md)** ✅
2. **[Lab 02: Automated Bulk User Creation](./Active-Directory-Toolkit/02-Bulk-User-Creation.md)** ✅
3. **[Lab 03: Privileged Group Membership Audit](./Active-Directory-Toolkit/03-Privileged-Group-Audit.md)** 🟡
4. **[Lab 04: Stale Account Identification & Disablement](./Active-Directory-Toolkit/04-Stale-Account-Cleanup.md)** ⚪
5. **[Lab 05: Automated OU Structure Deployment](./Active-Directory-Toolkit/05-OU-Restructuring.md)** ⚪
6. **[Lab 06: Password Policy Compliance Reporting](./Active-Directory-Toolkit/06-Password-Policy-Audit.md)** ⚪
7. **[Lab 07: Incident Response: Automated Account Lockout](./Active-Directory-Toolkit/07-User-Deprovisioning.md)** ⚪
8. **[Lab 08: GPO Inventory & Link Reporting](./Active-Directory-Toolkit/08-GPO-Report.md)** ⚪
9. **[Lab 09: DNS & Domain Controller Health Check](./Active-Directory-Toolkit/09-AD-Health-Check.md)** ⚪
10. **[Lab 10: Security Log Parsing: Brute Force Detection](./Active-Directory-Toolkit/10-Security-Log-Parsing.md)** ⚪

---

## 👨‍💻 About the Author
**Thomas Price**
*Cybersecurity Professional*

**Current Certifications:**
* GIAC Foundational Cybersecurity Technologies (**GFACT**)
* GIAC Security Essentials (**GSEC**)
* GIAC Security Operations Certified (**GSOC**)
* CompTIA **A+**
* *Currently pursuing CompTIA Security+ 701*

**Connect with me:**
* **GitHub:** [https://github.com/tp8888](https://github.com/tp8888)
* **Medium:** [Thomas Price on Medium](https://medium.com/@thomas_d_price)
