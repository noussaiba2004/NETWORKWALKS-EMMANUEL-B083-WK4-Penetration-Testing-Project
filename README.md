# 🏥 Mediroza Hospital Penetration Test

## 📖 Overview
This repository contains the methodology, attack chain, and final penetration testing report for a simulated Black-Box assessment of a healthcare web application (Mediroza General Hospital). The project demonstrates a full-chain exploit, starting from initial access via authentication bypass to the exfiltration of highly sensitive financial and patient data.

## 🎯 Objectives
- Assess the external security posture of the Mediroza web portal.
- Identify and exploit web vulnerabilities to escalate privileges.
- Compromise encrypted documents to assess password policies.
- Conduct forensic metadata analysis to discover hidden infrastructure exposures.

## 🛠️ Tools Used
- **OS:** Kali Linux
- **Web Exploitation:** Manual SQL Injection (SQLi) payload crafting
- **Cryptography / Cracking:** Networkwalks Password Cracker, Public wordlists (`rockyou.txt`)
- **Forensics:** ExifTool

## Attack Chain & Methodology

### 1. Initial Access: SQL Injection (SQLi)
The patient login portal lacked proper input sanitization. By injecting the boolean payload `admin' or '1'='1` into the authentication fields, the SQL syntax was broken, forcing a true condition. This bypassed the authentication mechanisms entirely and allowed the extraction of the backend `staff` database table, exposing employee names, emails, and salaries.

### 2. Cryptographic Attacks: Encrypted PDF Cracking
Three exfiltrated pathology reports (PDF format) were encrypted using Standard V2.3 (128-bit) encryption. An offline password cracking methodology was applied using the Networkwalks Password Cracker and standard public wordlists. The weak password policies allowed the documents to be decrypted:
- **Report 1 & 2:** Cracked using standard dictionary attacks, revealing trivial passwords (e.g., `123456`).
- **Report 3:** Cracked using the public `rockyou.txt` wordlist, revealing a predictable keyboard-walk pattern (`!@#$%^&`).

### 3. Information Disclosure: Metadata Forensics
After decrypting the pathology reports, `ExifTool` was used to extract the internal metadata of the PDF files. A developer comment (`DB backup moved to /old before site migration, do not delete`) was discovered in the `Comments` field. 

### 4. Data Exfiltration: Backup Exposure
Navigating to the hidden `/old` directory revealed an unsecured, publicly accessible database backup. Downloading this archive granted complete access to the hospital's internal shareholder details and full financial records, concluding the kill chain.

## Deliverables
- `Penetration_Testing_Report.pdf`: A comprehensive executive and technical report detailing the scope, findings, risk ratings, and actionable remediation steps. (See repository files).

## ⚠️ Disclaimer
*This project is a Capture The Flag (CTF) simulation. All methodologies were executed in a controlled, authorized educational environment. Do not use these techniques against targets without explicit written permission.*
