# Mediroza General Hospital
# Penetration Testing Report

## Client Information

| Item | Value |
|--------|--------|
| Client | Mediroza General Hospital |
| Assessment Type | Black-Box Penetration Test |
| Target | https://medirozahospital.com |
| Pentester | Anderson Edaiyo Uzezi |
| Report Classification | Confidential |
| Engagement Status | Complete |

---

# 1. Executive Summary

A black-box penetration test was conducted against Mediroza General Hospital to evaluate the security of its public-facing infrastructure, authentication mechanisms, sensitive data protection controls, and information management practices.

The assessment successfully identified multiple vulnerabilities across the application attack surface that allowed unauthorized access to protected resources and sensitive organizational information.

The attack originated from publicly accessible information disclosure weaknesses and progressed through exposed application resources, authentication vulnerabilities, weak document protection mechanisms, and sensitive data exposure.

Successful exploitation resulted in access to:

- Sensitive patient laboratory reports
- Internal employee salary records
- Organizational ownership information
- Corporate shareholder records
- Business-sensitive data

Several vulnerabilities could be chained together, significantly increasing the overall impact.

Based on the likelihood of exploitation and the sensitivity of the recovered information, the overall security posture of the target environment has been assessed as:

# Overall Risk Rating: CRITICAL

Immediate remediation is recommended.

---

# 2. Scope and Methodology

## Scope

| Item | Description |
|----------|----------|
| Target Domain | medirozahospital.com |
| Assessment Type | Black-Box |
| Application Scope | Public Web Application |
| Testing Scope | Authorized Security Testing |
| Social Engineering | Out of Scope |
| Denial of Service | Out of Scope |

---

## Methodology

The assessment was conducted in five phases.

### Phase 1 – Reconnaissance

Information gathering activities were performed to identify publicly available infrastructure information.

Activities included:

- WHOIS analysis
- DNS enumeration
- Technology fingerprinting
- Service discovery
- Header analysis

### Phase 2 – Application Mapping

The application attack surface was mapped to identify:

- Authentication portals
- User input locations
- Hidden resources
- Legacy content
- Sensitive files

### Phase 3 – Initial Access & Authentication Assessment

Authentication mechanisms were tested to determine whether unauthorized access could be obtained.

### Phase 4 – Data Extraction

Protected patient reports were evaluated to determine the effectiveness of their encryption and password controls.

### Phase 5 – Critical Data Exposure Assessment

Sensitive records discovered during earlier phases were analyzed to determine the extent and impact of information disclosure.

---

## Tools Used

| Tool | Purpose |
|--------|--------|
| WHOIS | Domain Intelligence |
| NSLookup | DNS Resolution |
| DNSRecon | DNS Enumeration |
| WhatWeb | Technology Fingerprinting |
| Curl | Header Analysis |
| Gobuster | Directory Enumeration |
| WAFW00F | WAF Identification |
| Browser | Manual Validation |
| Networkwalks Hash Calculator | PDF Hash Extraction |
| Networkwalks Password Cracker | Password Recovery |
| PDFCrack | Password Recovery |
| RockYou Wordlist | Dictionary Attack Testing |

---
# 3. Attack Chain Summary

The assessment demonstrated how multiple weaknesses could be chained together by an unauthenticated external attacker to achieve unauthorized access to sensitive organizational data.

The attack path progressed through reconnaissance, application mapping, authentication assessment, password recovery, and data exposure validation.

```text
Reconnaissance
      │
      ▼
Infrastructure Enumeration
      │
      ▼
robots.txt Disclosure
      │
      ▼
Discovery of Hidden Directory (/old/)
      │
      ▼
Directory Listing Enabled
      │
      ▼
Public Exposure of SQL Backup
      │
      ▼
Recovery of Internal Organizational Information
      │
      ▼
Authentication Testing
      │
      ▼
SQL Injection Authentication Bypass
      │
      ▼
Unauthorized Patient Portal Access
      │
      ▼
Retrieval of Protected Patient Reports
      │
      ▼
Offline Password Recovery
      │
      ▼
Access to Confidential Medical Records
      │
      ▼
Review of SQL Backup Contents
      │
      ▼
Recovery of Employee Salary Records
      │
      ▼
Recovery of Shareholder Ownership Records
```

The successful chaining of these findings significantly increased the impact of the engagement and resulted in unauthorized access to highly sensitive patient, employee, and corporate information.

---

# 4. Findings and Proof of Exploitation

---

# Finding F-001

## Registrar and Infrastructure Intelligence Disclosure

### Severity

**INFORMATIONAL**

### Description

Reconnaissance activities successfully identified domain registration information, DNS infrastructure, hosting information, and associated communication services.

The following information was identified:

- Domain registrar
- Public IP address
- DNS infrastructure
- Mail infrastructure
- Technology stack
- Security controls

### Evidence

- SS001-WHOIS.png

<img width="1360" height="662" alt="SS001-WHOIS" src="https://github.com/user-attachments/assets/3f5710d8-51e6-470a-8071-c1d7a875d6d3" />











- SS003-NSLOOKUP.png




<img width="1360" height="662" alt="SS003-NSLOOKUP" src="https://github.com/user-attachments/assets/bdfec2f5-85e1-4048-9c3e-e8c3bd659bd8" />







- SS006-DNSRECON.png


<img width="1360" height="662" alt="SS006-DNSRECON" src="https://github.com/user-attachments/assets/56c264f8-ba9d-4a22-80b1-fb9d015de849" />












### Impact

This information does not directly compromise the application but provides valuable intelligence for attacker reconnaissance and attack planning.

---

# Finding F-002

## Server Technology and Header Disclosure

### Severity

**LOW**

### Description

Technology fingerprinting confirmed the use of LiteSpeed Web Server technology and exposed implementation details through HTTP response headers.

### Evidence

- SS002-WHATWEB.png



<img width="1360" height="662" alt="SS002-WHATWEB" src="https://github.com/user-attachments/assets/03b0ac47-0ac2-4296-93b6-74c6002e6ac3" />












- SS004-CURL-I.png



<img width="1360" height="662" alt="SS004-CURL -I" src="https://github.com/user-attachments/assets/ba97d18d-e5db-4f6e-8674-86aef0a60cba" />











### Information Identified

```text
Server: LiteSpeed
x-turbo-charged-by: LiteSpeed
HTTP/2 Enabled
```

### Impact

Technology disclosure assists attackers in identifying platform-specific attack vectors and tailoring subsequent reconnaissance efforts.

---

# Finding F-003

## Missing Vulnerability Disclosure Policy

### Severity

**INFORMATIONAL**

### Description

No security.txt file was present under:

```text
/.well-known/security.txt
```

### Evidence




- SS009-SECURITYTXT.png




<img width="1360" height="662" alt="SS009-SECURITYTXT" src="https://github.com/user-attachments/assets/2ed99586-e3df-416e-9e1a-e89545114774" />










### Impact

The absence of a formal vulnerability disclosure policy may delay responsible reporting and remediation of future security issues.

---

# Finding F-004

## Information Disclosure via robots.txt

### Severity

**LOW**

### Description

The robots.txt file disclosed sensitive internal application paths that were not intended for public discovery.

### Disclosed Paths

```text
/patient/
/staff/
/old/
```

### Evidence



- SS007-ROBOTS.png




<img width="1360" height="662" alt="SS007B-ROBOTS" src="https://github.com/user-attachments/assets/b559dc47-06e9-4521-bc2c-40dcfc0c7510" />








### Impact

Discovery of the hidden `/old/` directory directly contributed to the identification of the most critical vulnerability discovered during the engagement.

---

# Finding F-005

## Public Database Backup Exposure

### Severity

**CRITICAL**

### Description

Application mapping identified a publicly accessible SQL backup file located within a directory exposed through robots.txt.

### Affected Resource

```text
/old/mediroza_db_backup_2019.sql
```

### Proof of Exploitation

```text
robots.txt
      ↓
Discovery of /old/
      ↓
Open Directory Listing
      ↓
Exposure of SQL Backup
      ↓
Download Without Authentication
      ↓
Sensitive Data Disclosure
```

### Evidence

- SS011B-OLD-DIRECTORY.png

<img width="1366" height="768" alt="OLD PAGE" src="https://github.com/user-attachments/assets/66cddf84-5f83-4012-b0ed-3710eec0f306" />






- SS011C-DATABASE-BACKUP.png










<img width="1366" height="768" alt="SS017-DATABASE-DIRECTORY" src="https://github.com/user-attachments/assets/bdbdfe71-c189-489c-86e9-f05d318d731e" />



### Impact

The exposed backup contained:

- Internal HR information
- Employee records
- Salary data
- Shareholder records
- Corporate ownership information
- Organizational data

### Root Cause

- Sensitive backup stored within web root
- Directory indexing enabled
- No authentication controls protecting backup files
- Information disclosure through robots.txt

---

# Finding F-006

## Authentication Bypass via SQL Injection

### Severity

**CRITICAL**

### Affected Endpoint

```text
/patient/login.php
```

### Description

Authentication controls protecting the Patient Portal were vulnerable to SQL Injection.

Testing demonstrated that authentication could be bypassed, resulting in unauthorized access to the Patient Portal without legitimate credentials.

### Evidence

- SS010C-PATIENT-PORTAL-LOGIN.png

<img width="1366" height="768" alt="PATIENT PORTAL 11" src="https://github.com/user-attachments/assets/ec754e28-9498-44e3-89af-8fca5c33cbde" />







- SS014-PATIENT-LOGIN.png

<img width="1366" height="768" alt="PATIENT LOGIN NOW" src="https://github.com/user-attachments/assets/6e5b3acf-13ae-4d52-abae-728b333444fe" />



















- SS014B-PDF-PATIENT-DISCOVERED.png



<img width="1366" height="768" alt="PDF PATIENT DISCOVRED" src="https://github.com/user-attachments/assets/a40251e3-aa08-40ba-88d4-44039e96fee7" />





### Impact

Successful exploitation resulted in:

- Authentication bypass
- Unauthorized patient portal access
- Access to downloadable patient laboratory reports
- Circumvention of application security controls

### Root Cause

- Insufficient input validation
- Lack of parameterized SQL queries
- Insecure authentication implementation

---
# Finding F-007

## Staff Portal Authentication Assessment

### Severity

INFORMATIONAL

### Affected Endpoint

```text
/staff/login.php
```

### Status

Authentication bypass not achieved.

### Description

The Staff Portal was identified during the Application Mapping phase and selected for authentication testing due to its potential access to internal hospital resources and administrative functionality.

The authentication mechanism was evaluated using the same controlled testing methodology applied to the Patient Portal.

Testing focused on:

- Authentication logic validation
- Input handling
- Common authentication bypass techniques
- Session establishment behavior
- Access control verification

No successful authentication bypass, unauthorized access, or session compromise was achieved during testing.

### Evidence

#### Evidence 1 – Staff Portal Login Interface

SS015-STAFF-LOGIN-FAIL.png


### Validation Result

✅ Authentication mechanism identified

✅ Login portal accessible

✅ Authentication testing completed

✅ No successful authentication bypass achieved

✅ No unauthorized access obtained

### Security Impact

No exploitable vulnerability was confirmed within the Staff Portal authentication mechanism during the scope of this assessment.

This result indicates the Staff Portal provided stronger resistance to the testing methodology used during the engagement when compared to the Patient Portal.

### Analyst Assessment

While no vulnerability was identified within the Staff Portal during testing, the portal remains a high-value target due to its likely access to internal administrative and employee resources.

Future assessments should include:

- Advanced - Session management assessment
- Authorization testing
- Business logic review





# Finding F-008

## Weak Password Protection on Patient Reports

### Severity

**HIGH**

### Description

Three password-protected patient reports were obtained during the engagement and assessed for resistance against offline password recovery attacks.

### Patient Report 1

#### Methodology

```text
Protected PDF
      ↓
Networkwalks Hash Calculator
      ↓
Hash Extraction
      ↓
Networkwalks Password Cracker
      ↓
Password Recovery
      ↓
Successful Access
```


SS03-HASH-CALCULATOR

Networkwalks Hash Calculator successfully extracted the password hash from the encrypted patient_report 1 – Password Recovery

<img width="1366" height="768" alt="SS03-COPY-HASH-PATIENT-PDF-1" src="https://github.com/user-attachments/assets/8c08a68f-0c10-40e7-a15c-74886f4eea30" />




SS04-PASSWORD-CRACKER

Networkwalks Password Cracker successfully recovered the document password from the extracted hash.

<img width="1366" height="768" alt="SS07-PDF-PASSWORD-FOR-PATIENT-1-CRACKED-SUCESSFULLY" src="https://github.com/user-attachments/assets/9281c0e5-2406-4bd2-ac81-301bcdb20749" />


SS05-ACCESSING-PDF1



<img width="1366" height="768" alt="SS08-ACCESSING-PDF-1-PATIENT_REPORT" src="https://github.com/user-attachments/assets/8c4b28df-f11b-4fa2-8eae-4128612e1101" />



SS06-PDF-1-ACCESSED


Patient Report 1 successfully decrypted and opened using the recovered password.

<img width="1366" height="768" alt="SS09-PDF-1-PATIENT _REPORT- ACCESSED SUCESSFULY" src="https://github.com/user-attachments/assets/61219fcc-ad05-4d3b-a1af-4bfa5a46cfec" />





### Patient Report 2

#### Methodology

```text
Protected PDF
      ↓
Networkwalks Hash Calculator
      ↓
Hash Extraction
      ↓
Networkwalks Password Cracker
      ↓
Password Recovery
      ↓
Successful Access
```




SS03-HASH-CALCULATOR

Networkwalks Hash Calculator successfully extracted the password hash from the encrypted patient_report 2 – Password Recovery

<img width="1366" height="768" alt="SS03-COPY-HASH-PATIENT-PDF-2" src="https://github.com/user-attachments/assets/725b1bf2-609d-4754-9436-76b341982dff" />




SS04-PASSWORD-CRACKER

Networkwalks Password Cracker successfully recovered the document password from the extracted hash.



<img width="1366" height="768" alt="SS07-PDF-PASSWORD-FOR-PATIENT-2-CRACKED-SUCESSFULY" src="https://github.com/user-attachments/assets/435503c9-5679-49a2-8041-c7fa6dfe8b08" />



SS05-ACCESSING-PDF-2

<img width="1366" height="768" alt="SS08-ACCESSING-PDF-2-PATIENT_REPORT" src="https://github.com/user-attachments/assets/2bedd9f0-0619-4504-aa8f-4c63ef1357f8" />




SS06-PDF-2-ACCESSED


Patient Report 2 successfully decrypted and opened using the recovered password.





<img width="1366" height="768" alt="SS09-PDF-2-PATIENT_REPORT-ACCESSED-SUCESSFULY" src="https://github.com/user-attachments/assets/e3e10e25-4dee-4e5d-a247-accb9b62bbc9" />


### Patient Report 3

#### Methodology

```text
Protected PDF
      ↓
PDFCrack
      ↓
RockYou Wordlist
      ↓
Password Recovery
      ↓
Successful Access
```

SS01_PDFCRACK_ROCKYYOU_PATIENT_REPORT_3

PDFCrack successfully performed offline password recovery using the RockYou wordlist


<img width="1360" height="662" alt="SS01_PDFCRACK_ROCKYYOU_PATIENT_REPORT_3" src="https://github.com/user-attachments/assets/e9c04159-6406-4020-87dc-7d3a832d5fb0" />



SS02-ACCESSING-PDF-3-PATIENT_REPORT

Recovered password identified and validated

<img width="1366" height="768" alt="SS02-ACCESSING-PDF-3-PATIENT_REPORT" src="https://github.com/user-attachments/assets/33244ef5-f5cf-4426-9b73-35b9ec85d4b6" />



SS03-PDF-3-PATIENT_REPORT-ACCESSED


<img width="1366" height="768" alt="SS03-PDF-3-PATIENT_REPORT-ACCESSED" src="https://github.com/user-attachments/assets/7bb26c35-0a6c-478b-8699-0c85a60b01ae" />




### Impact

Successful password recovery allowed unauthorized access to confidential patient medical information.

### Root Cause

- Weak password selection
- Predictable passwords
- Lack of password complexity requirements

---

# Finding F-009

## Employee Salary Information Exposure

### Severity

**CRITICAL**

### Description

The exposed SQL backup contained sensitive payroll and employment information belonging to hospital personnel.

### Sample Recovered Records

## Employee Salary Information Exposure

| Employee | Position | Department | Annual Salary |
| :--- | :--- | :--- | :--- |
| Dr. Rajesh Naidoo | Chief Pathologist | Diagnostics Lab | R 1,656,000 |
| Sarah Botha | Chief Financial Officer | Finance | R 1,824,000 |
| Dr. Johan van der Merwe | Medical Director | Management | R 1,920,000 |
| Dr. Anita Naicker | Consultant Cardiologist | Cardiology | R 1,584,000 |
| Dr. Ahmed Kara | Consultant Physician | Internal Medicine | R 1,536,000 |
| Dr. Yusuf Cassim | Senior Registrar | Emergency & Trauma | R 888,000 |
| Michael Roberts | HR Director | Human Resources | R 1,152,000 |
| Susan Pretorius | HR Officer | Human Resources | R 384,000 |
| Jameel Malik | IT Systems Administrator | IT | R 696,000 |
| Thabo Molefe | Network Engineer | IT | R 552,000 |
| Nomvula Khumalo | Registered Nurse | Emergency & Trauma | R 408,000 |
| Lerato Mokoena | Registered Nurse | Pediatrics | R 396,000 |
| Bongani Ndlovu | Registered Nurse | Cardiology | R 420,000 |
| Zanele Mahlangu | Nursing Sister | Theatre | R 504,000 |
| Kagiso Sithole | Pharmacist | Pharmacy | R 732,000 |
| Naledi Zulu | Pharmacy Assistant | Pharmacy | R 312,000 |
| Themba Nkosi | Radiographer | Radiology | R 528,000 |
| Palesa Radebe | Radiographer | Radiology | R 516,000 |
| Deepak Pillay | Lab Technologist | Diagnostics Lab | R 492,000 |
| Kavitha Govender | Lab Technician | Diagnostics Lab | R 420,000 |
| Dr. Suresh Moodley | Consultant Radiologist | Radiology | R 1,560,000 |
| Dr. Fatima Patel | Pediatrician | Pediatrics | R 1,416,000 |
| Nisha Singh | Physiotherapist | Rehabilitation | R 576,000 |
| Dr. Vikram Chetty | Anaesthetist | Theatre | R 1,620,000 |
| David Smith | Facilities Manager | Operations | R 624,000 |
| Karen O'Connor | Billing Administrator | Finance | R 348,000 |
| James Wilson | Security Supervisor | Operations | R 324,000 |
| Linda Fourie | Receptionist | Front Office | R 228,000 |
| Peter van Wyk | Procurement Officer | Supply Chain | R 456,000 |
| Andile Mbeki | Ward Clerk | Administration | R 252,000 |

---
### Evidence

- EMP-01.png

<img width="1363" height="768" alt="EMP-01" src="https://github.com/user-attachments/assets/16d0ee97-f1c3-4205-8466-16141acb22b0" />















### Impact

Exposure of employee salary information may result in:

- Privacy violations
- Targeted social engineering
- Employee disputes
- Financial profiling
- Reputational damage

---

# Finding F-010

## Shareholder Ownership Information Exposure

### Severity

**CRITICAL**

### Description

Analysis of the database backup identified confidential shareholder ownership information.

### Sample Recovered Records

| Shareholder | Ownership % | Shares Held | Share Class |
|-------------|-------------|-------------|-------------|
| Dr. Rajesh Naidoo | 18.0% | 180,000 | Ordinary |
| Cedar Health Holdings (Pty) Ltd | 15.0% | 150,000 | Ordinary |
| Dr. Johan van der Merwe | 12.0% | 120,000 | Ordinary |
| Reddy Family Trust | 11.0% | 110,000 | Ordinary |
| Thabo Molefe | 10.0% | 100,000 | Ordinary |
| Sarah Botha | 9.0% | 90,000 | Ordinary |
| Dr. Ahmed Kara | 8.0% | 80,000 | Preferential |
| Naledi Zulu | 7.0% | 70,000 | Ordinary |
| Michael Roberts | 6.0% | 60,000 | Ordinary |
| Dr. Vikram Chetty | 4.0% | 40,000 | Preferential |

---
### Evidence

- SHR-01.png




<img width="1366" height="426" alt="SHR-01" src="https://github.com/user-attachments/assets/d3442f3f-5e41-447e-bb50-46e6d9e462c5" />










### Impact

Exposure of shareholder information enables:

- Corporate intelligence gathering
- Ownership structure analysis
- Investor profiling
- Strategic business intelligence collection
- Increased targeting of key stakeholders

---

# 5. Risk Rating Summary

| Finding ID | Finding | Severity |
|------------|----------|----------|
| F-001 | Registrar and Infrastructure Intelligence Disclosure | Informational |
| F-002 | Server Technology and Header Disclosure | Low |
| F-003 | Missing Vulnerability Disclosure Policy | Informational |
| F-004 | robots.txt Information Disclosure | Low |
| F-005 | Public Database Backup Exposure | Critical |
| F-006 | Authentication Bypass via SQL Injection | Critical |
| F-007 | Staff Portal Authentication Assessment  | Informational  |
| F-008 | Weak Password Protection on Patient Reports | High |
| F-009 | Employee Salary Information Exposure | Critical |
| F-010 | Shareholder Ownership Information Exposure | Critical |

## Overall Risk Assessment

### Overall Rating: CRITICAL

The assessment identified multiple vulnerabilities that could be chained together to achieve significant compromise of confidential organizational assets.

The combination of:

- Information disclosure
- Public backup exposure
- Authentication bypass
- Weak password protection
- Employee data exposure
- Shareholder data exposure

resulted in a Critical overall risk rating.

Immediate remediation is recommended.
---

# 5. Recommendations and Remediation

## Immediate Actions

1. Remove exposed SQL backup files from publicly accessible locations.
2. Disable directory indexing on all web-accessible directories.
3. Restrict access to legacy content and resources.
4. Review historical access logs for unauthorized activity.
5. Perform an internal breach impact assessment.

---

## Authentication Security

1. Review and remediate authentication vulnerabilities.
2. Implement secure coding practices.
3. Validate and sanitize user input.
4. Conduct application security testing following remediation.

---

## Data Protection

1. Replace weak passwords protecting sensitive files.
2. Enforce strong passphrase requirements.
3. Store sensitive records within controlled environments.
4. Encrypt sensitive information using enterprise-grade protections.

---

## Long-Term Security Improvements

1. Implement secure backup management procedures.
2. Introduce Data Loss Prevention (DLP) controls.
3. Establish formal data classification standards.
4. Conduct routine penetration testing.
5. Implement continuous security monitoring.

---

# Conclusion

The assessment identified multiple vulnerabilities that could be chained together to achieve significant compromise of organizational and patient information.

Critical findings included the exposure of a public database backup, authentication weaknesses, insufficient protection of patient reports, and disclosure of sensitive employee and shareholder records.

The combination of these findings results in a Critical overall risk level and requires immediate remediation to reduce organizational exposure.

---

# Assessment Outcome

✅ Reconnaissance Completed

✅ Application Mapping Completed

✅ Authentication Testing Completed

✅ Data Extraction Completed

✅ Data Exposure Assessment Completed

✅ Critical Risks Identified

✅ Remediation Recommendations Provided

✅ Penetration Test Completed







# Appendix A – Sensitive Validation Evidence

## Confidential Evidence

The contents of this appendix are included solely for assessment validation purposes and contain sensitive information recovered during authorized testing.

---

## A.1 Authentication Testing

### Authentication Bypass Payload

| Field | Value |
|---------|---------|
| Username Payload | admin'-- |
| Password Value Used During Testing | 12345 |

### Result

✅ Authentication Bypass Successful

✅ Patient Portal Access Obtained

---

## A.2 Patient Report Recovery

### Patient Report 1

| Item | Value |
|---------|---------|
| Status | Password Recovered |
| Recovered Password | 123456 |

### Result

✅ Patient Report 1 Accessed

---

### Patient Report 2

| Item | Value |
|---------|---------|
| Status | Password Recovered |
| Recovered Password | password |

### Result

✅ Patient Report 2 Accessed


---

### Patient Report 3

| Item | Value |
|---------|---------|
| Status | Password Recovered |
| Recovered Password | !@#$%^& |

### Result

✅ Patient Report 3 Accessed

---

## A.3 Validation Summary

| Asset | Access Obtained |
|---------|---------|
| Patient Report 1 | ✅ |
| Patient Report 2 | ✅ |
| Patient Report 3 | ✅ |
| Employee Records | ✅ |
| Shareholder Records | ✅ |


---
## REPOSITORY STRUCTURE
```text
Mediroza-Penetration-Test/
│
├── README.md
│
├── report/
│   ├── 01-Executive-Summary.md
│   ├── 02-Scope-and-Methodology.md
│   ├── 03-Attack-Chain-Summary.md
│   ├── 04-Findings-and-Proof-of-Exploitation.md
│   ├── 05-Risk-Rating-Summary.md
│   ├── 06-Recommendations-and-Remediation.md
│   ├── 07-Conclusion.md
│   └── Appendix-A-Sensitive-Validation-Evidence.md
│
├── evidence/
│
│   ├── reconnaissance/
│   │   ├── SS001-WHOIS.png
│   │   ├── SS002-WHATWEB.png
│   │   ├── SS003-NSLOOKUP.png
│   │   ├── SS004-CURL-I.png
│   │   ├── SS005-WAFW00F.png
│   │   ├── SS006-DNSRECON.png
│   │   └── SS009-SECURITYTXT.png
│   │
│   ├── application-mapping/
│   │   ├── SS007-ROBOTS.png
│   │   ├── SS008A-SITEMAP.png
│   │   ├── SS08B-SITE-MAP.png
│   │   
│   │   ├── SS012-GOBUSTER.png
│   │   └── SS013-WHATWEB-FORMS.png
│   │
│   ├── initial-access/
│   │   ├── SS014-PATIENT-LOGIN.png
│   │   ├── SS015-STAFF-LOGIN.png
│   │   ├── SS016-OLD-DIRECTORY.png
│   │   ├── SS017-DATABASE-BACKUP.png
│   │   ├── SS018-PATIENT-PORTAL.png
│   │   ├── PDF-PATIENT-DISCOVERED.png
│   │   
│   │
│   ├── milestone-2/
│   │   │
│   │   ├── patient-report-1/
│   │   │   ├── SS03-COPY-HASH-PATIENT-PDF-1.png
│   │   │   ├── SS07-PDF-PASSWORD-FOR-PATIENT-1.png
│   │   │   ├── SS08-ACCESSING-PDF-1.png
│   │   │   └── SS09-PDF-1-ACCESSED.png
│   │   │
│   │   ├── patient-report-2/
│   │   │   ├── SS03-COPY-HASH-PATIENT-PDF-2.png
│   │   │   ├── SS07-PDF-PASSWORD-FOR-PATIENT-2.png
│   │   │   ├── SS08-ACCESSING-PDF-2.png
│   │   │   └── SS09-PDF-2-ACCESSED.png
│   │   │
│   │   └── patient-report-3/
│   │       ├── SS01-PDFCRACK-ROCKYOU.png
│   │       ├── SS02-ACCESSING-PDF-3.png
│   │       └── SS03-PDF-3-ACCESSED.png
│   │
│   └── milestone-3/
│       ├── EMP-01.png
│       └── SHR-01.png
│
├── findings/
│   ├── F-001-Infrastructure-Intelligence.md
│   ├── F-002-Server-Technology-Disclosure.md
│   ├── F-003-Missing-SecurityTXT.md
│   ├── F-004-RobotsTXT-Disclosure.md
│   ├── F-005-Public-Database-Backup.md
│   ├── F-006-SQL-Injection-Authentication-Bypass.md
│   ├── F-007-Weak-PDF-Password-Protection.md
│   ├── F-008-Employee-Salary-Exposure.md
│   └── F-009-Shareholder-Exposure.md
│
├── milestones/
│   ├── Milestone-1-Reconnaissance.md
│   ├── Milestone-1-Application-Mapping.md
│   ├── Milestone-1-Initial-Access.md
│   ├── Milestone-2-Data-Extraction.md
│   ├── Milestone-3-Critical-Data-Exposure.md
│   └── Milestone-4-Final-Report.md
│
├── assets/
│   ├── cover-image.png
│   ├── attack-chain.png
│   └── risk-matrix.png
│
└── LICENSE
```


---
## 🔗 Engagement Phase Repositories

| Milestone / Phase | Repository Link | Scope Covered |
| :--- | :--- | :--- |
| **Milestone 1.1 — Reconnaissance** | [RECONNAISSANCE-AND-INTELLIGENCE-COLLECTION](https://github.com/Zezanderson98/RECONNAISSANCE-AND-INTELLIGENCE-COLLECTION) | WHOIS, DNS, WAF detection, headers, `security.txt` |
| **Milestone 1.2 — Application Mapping** | [APPLICATION-MAPPING-AND-EXPOSED-ENTRY-POINTS](https://github.com/Zezanderson98/APPLICATION-MAPPING-AND-EXPOSED-ENTRY-POINTS) | `robots.txt`, sitemaps, directory brute-forcing, forms |
| **Milestone 1.3 — Initial Access & Auth** | [INITIAL-ACCESS-AND-AUTHENTICATION-TESTING](https://github.com/Zezanderson98/INITIAL-ACCESS-AND-AUTHENTICATION-TESTING) | Exposed DB backup, legacy directories, SQLi auth bypass |
| **Milestone 2 — Data Extraction & Recovery** | [DATA-EXTRACTION-AND-RECOVERY-OF-ENCRYPTED-PATIENT-RECORDS](https://github.com/Zezanderson98/DATA-EXTRACTION-AND-RECOVERY-OF-ENCRYPTED-PATIENT-RECORDS) | Encrypted patient PDF hash extraction & password recovery |
| **Milestone 3 — Critical Data Exposure** | [CRITICAL-DATA-EXPOSURE-ASSESSMENT](https://github.com/Zezanderson98/CRITICAL-DATA-EXPOSURE-ASSESSMENT) | Employee salary records & shareholder data exposure |

---


---

## 🔍 Summary of Findings

| ID | Finding | Severity | Phase Repository (Step-by-Step) |
| :--- | :--- | :--- | :--- |
| **F-001** | Infrastructure Intelligence | Informational | [Reconnaissance Repo](https://github.com/Zezanderson98/RECONNAISSANCE-AND-INTELLIGENCE-COLLECTION)  |
| **F-002** | Server Technology Disclosure | Low | [Reconnaissance Repo](https://github.com/Zezanderson98/RECONNAISSANCE-AND-INTELLIGENCE-COLLECTION) | |
| **F-003** | Missing `security.txt` | Low / Informational | [Application Mapping Repo] (https://github.com/Zezanderson98/APPLICATION-MAPPING-AND-EXPOSED-ENTRY-POINTS)|
| **F-004** | `robots.txt` Sensitive Path Disclosure | Low / Informational | [Application Mapping Repo](https://github.com/Zezanderson98/APPLICATION-MAPPING-AND-EXPOSED-ENTRY-POINTS) |
| **F-005** | Publicly Accessible Database Backup (`/old/mediroza_db_backup_2019.sql`) | **CRITICAL** | [Application Mapping Repo](https://github.com/Zezanderson98/APPLICATION-MAPPING-AND-EXPOSED-ENTRY-POINTS)/ [Initial Access Repo](https://github.com/Zezanderson98/INITIAL-ACCESS-AND-AUTHENTICATION-TESTING) |
| **F-006** | SQL Injection Authentication Bypass (`/patient/login.php`) | **CRITICAL** | [Initial Access Repo](https://github.com/Zezanderson98/INITIAL-ACCESS-AND-AUTHENTICATION-TESTING) |
| **F-007** | Staff Portal Authentication Assessment | Informational | [Initial Access Repo](https://github.com/Zezanderson98/INITIAL-ACCESS-AND-AUTHENTICATION-TESTING) |
| **F-008** | Weak PDF Password Protection (Patient Reports 1–3) | **HIGH** | [Data Extraction & Recovery Repo] (https://github.com/Zezanderson98/DATA-EXTRACTION-AND-RECOVERY-OF-ENCRYPTED-PATIENT-RECORDS)|
| **F-009** | Employee Salary Information Exposure | **CRITICAL** | [Critical Data Exposure Repo] (https://github.com/Zezanderson98/CRITICAL-DATA-EXPOSURE-ASSESSMENT) |
| **F-010** | Shareholder Information Exposure | **CRITICAL** | [Critical Data Exposure Repo](https://github.com/Zezanderson98/CRITICAL-DATA-EXPOSURE-ASSESSMENT) |


---

## ⚠️ Disclaimer

This assessment was performed strictly within an authorized educational/lab environment for defensive security reporting and portfolio demonstration purposes.

