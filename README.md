# 💻 PowerShell Labs & Upskilling

Welcome to my central repository for PowerShell automation and security research. This project documents my transition from a career in automotive technology into **Cybersecurity**, focusing on leveraging PowerShell for system administration, threat hunting, and infrastructure management.

## 🎯 Project Goals
The objective of this repository is to build a library of reusable scripts and detailed "Threat-Brief" style documentation that demonstrates proficiency in:
* **Enterprise Administration:** Managing Active Directory objects and Group Policy at scale.
* **Security Automation:** Parsing logs and identifying Indicators of Compromise (IoCs).
* **System Hardening:** Automating security configurations based on industry benchmarks.

---

## 🛠️ Lab Environment: The "Golden Image" Workflow
All scripts in this repository are developed and tested within a dedicated **Windows Enterprise VM** in Oracle VirtualBox. 

To ensure a safe and repeatable testing environment, I utilize the following workflow:
1. **Base Configuration:** A clean install of Windows Enterprise with RSAT (Remote Server Administration Tools) installed.
2. **Snapshots:** A "Golden Image" snapshot is taken before any script execution.
3. **Rollback:** After testing destructive or high-impact scripts (like bulk user deletion or policy changes), the VM is rolled back to the clean state.

---

## 📂 Repository Roadmap

### 🏢 [Active Directory PowerShell Toolkit](./Active-Directory-Toolkit/)
Focuses on automating the administration and security auditing of an AD environment.
* **Lab 01:** [Environment & Module Verification](./Active-Directory-Toolkit/01-Environment-Verification.md)
* **Status:** In Progress 🟡

### 🔍 Security Automation
Focuses on log analysis, incident response scripts, and honeypot data parsing.
* **Status:** Upcoming ⚪

### 🛡️ System Hardening
Automated application of security benchmarks (CIS/STIGs) to Windows endpoints.
* **Status:** Upcoming ⚪

---

## 👨‍💻 About the Author
**Thomas Price** *Cybersecurity Professional | Former Honda Master Technician*

**Current Certifications:**
* GIAC Foundational Cybersecurity Technologies (**GFACT**)
* GIAC Security Essentials (**GSEC**)
* GIAC Security Operations Certified (**GSOC**)
* CompTIA **A+**
* *Currently pursuing CompTIA Security+ 701*

**Connect with me:**
* **GitHub:** [https://github.com/tp8888](https://github.com/tp8888)
* **Medium:** [Thomas Price on Medium](https://medium.com/@thomas_d_price)
