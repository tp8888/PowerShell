# 💻 PowerShell Labs & Upskilling

Welcome to my central repository for PowerShell automation and security research. This project documents my transition into **Cybersecurity**, focusing on leveraging PowerShell for system administration, threat hunting, and infrastructure management.

## 🎯 Project Goals
The objective of this repository is to build a library of reusable scripts and detailed "Threat-Brief" style documentation that demonstrates proficiency in:
* **Enterprise Administration:** Managing Active Directory objects and Group Policy at scale.
* **Security Automation:** Parsing logs and identifying Indicators of Compromise (IoCs).
* **System Hardening:** Automating security configurations based on industry benchmarks.

---

## 🛠️ Lab Environment: The "Golden Image" Workflow
All scripts in this repository are developed and tested within a dedicated **Windows Enterprise VM** in Oracle VirtualBox. 

To ensure a safe and repeatable testing environment, I utilize the following workflow:
1. **Base Configuration:** A clean install of Windows Enterprise with RSAT installed.
2. **Snapshots:** A "Golden Image" snapshot is taken before any script execution.
3. **Rollback:** After testing high-impact scripts, the VM is rolled back to the clean state.

---

## 📂 Active Directory Toolkit: 10-Lab Roadmap
I am currently working through a 10-lab curriculum focused on Enterprise Directory management and security.

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
*Cybersecurity Professional | Former Honda Master Technician*

**Current Certifications:**
* GIAC Foundational Cybersecurity Technologies (**GFACT**)
* GIAC Security Essentials (**GSEC**)
* GIAC Security Operations Certified (**GSOC**)
* CompTIA **A+**
* *Currently pursuing CompTIA Security+ 701*

**Connect with me:**
* **GitHub:** [https://github.com/tp8888](https://github.com/tp8888)
* **Medium:** [Thomas Price on Medium](https://medium.com/@thomas_d_price)
