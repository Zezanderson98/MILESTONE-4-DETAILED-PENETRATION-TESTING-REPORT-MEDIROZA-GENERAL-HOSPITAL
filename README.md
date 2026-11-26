# Mediroza General Hospital
# Penetration Testing Report

## Client Information

| Item | Value |
|--------|--------|
| Client | Mediroza General Hospital |
| Assessment Type | Black-Box Penetration Test |
| Target | https://medirozahospital.com |
| Penester | Anderson Edaiyo Uzezi |
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

# 3. Findings and Proof of Exploitation

---

# Finding F-001

## Information Disclosure via robots.txt

### Severity

LOW

### Description

The robots.txt file disclosed internal application paths including:

- /patient/
- /staff/
- /old/

The disclosed legacy directory ultimately contributed to the discovery of additional sensitive resources.

### Evidence

- SS007-ROBOTS.png

### Impact

The information disclosure provided valuable intelligence regarding the application structure and increased the efficiency of subsequent attack-surface discovery activities.

---

# Finding F-002

## Public Database Backup Exposure

### Severity

CRITICAL

### Description

Application mapping identified an exposed directory listing within:

```text
/old/
```

The directory exposed a publicly downloadable SQL backup file:

```text
mediroza_db_backup_2019.sql
```

No authentication or authorization controls protected access to the file.

### Proof of Exploitation

```text
robots.txt
 ↓
/old/
 ↓
Directory Listing
 ↓
Database Backup Exposure
 ↓
Sensitive Information Disclosure
```

### Evidence

- SS016-OLD-DIRECTORY.png
- SS017-DATABASE-BACKUP.png
- OLD PAGE.png

### Impact

The exposed backup contained organizational information, employee records, salary information, and shareholder data.

---

# Finding F-003

## Authentication Bypass

### Severity

CRITICAL

### Description

Testing identified a vulnerability within the Patient Portal authentication mechanism that allowed authentication controls to be bypassed.

Successful exploitation resulted in unauthorized access to the Patient Portal and restricted functionality.

### Evidence

- SS014-PATIENT-LOGIN.png
- SS018-PATIENT-PORTAL.png

### Impact

The vulnerability allowed unauthorized access to protected patient resources and downloadable reports.

---

# Finding F-004

## Weak Password Protection on Patient Reports

### Severity

HIGH

### Description

Three password-protected PDF laboratory reports were recovered and assessed.

Password protection was insufficient to prevent offline password recovery.

### Proof of Exploitation

#### Patient Reports 1 

```text
PDF
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

#### Patient Reports 2

```text
PDF
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







#### Patient Report 3

```text
PDF
 ↓
PDFCrack
 ↓
RockYou Wordlist
 ↓
Password Recovery
 ↓
Successful Access
```

### Evidence

- Password recovery screenshots
- Hash extraction screenshots
- Opened report screenshots

### Impact

Successful password recovery provided unrestricted access to confidential medical information.

---

# Finding F-005

## Critical Exposure of Employee and Shareholder Records

### Severity

CRITICAL

### Description

Analysis of the exposed database backup revealed confidential organizational information relating to employee compensation and corporate ownership.

---

## Employee Salary Records

### Sample Recovered Records

| Employee | Position | Department | Annual Salary |
|-----------|-----------|-----------|-----------|
| Dr. Suresh Moodley | Consultant Radiologist | Radiology | 130,000 |
| Dr. Vikram Chetty | Anaesthetist | Theatre | 135,000 |
| Dr. Fatima Patel | Pediatrician | Pediatrics | 118,000 |
| David Smith | Facilities Manager | Operations | 52,000 |
| Nisha Singh | Physiotherapist | Rehabilitation | 48,000 |
| Radebe | Radiographer | Radiology | 43,000 |
| Deepak Pillay | Lab Technologist | Diagnostics Lab | 41,000 |
| Peter van Wyk | Procurement Officer | Supply Chain | 38,000 |
| Kavitha Govender | Lab Technician | Diagnostics Lab | 35,000 |
| Karen O'Connor | Billing Administrator | Finance | 29,000 |
| James Wilson | Security Supervisor | Operations | 27,000 |
| Andile Mbeki | Ward Clerk | Administration | 21,000 |
| Linda Fourie | Receptionist | Front Office | 19,000 |

---

## Shareholder Ownership Records

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

### Impact

The exposed information permits unauthorized parties to obtain intelligence relating to:

- Employee compensation
- Organizational structure
- Corporate ownership
- Investor relationships
- Business governance

The disclosure creates significant privacy, financial, and reputational risks.

---

# 4. Risk Rating Summary

| Finding | Severity |
|----------|----------|
| Information Disclosure via robots.txt | Low |
| Public Database Backup Exposure | Critical |
| Authentication Bypass | Critical |
| Weak Password Protection on Patient Reports | High |
| Employee & Shareholder Record Exposure | Critical |

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







# Appendix B – Sensitive Validation Evidence

## Confidential Evidence

The contents of this appendix are included solely for assessment validation purposes and contain sensitive information recovered during authorized testing.

---

## B.1 Authentication Testing

### Authentication Bypass Payload

| Field | Value |
|---------|---------|
| Username Payload | admin'-- |
| Password Value Used During Testing | 12345 |

### Result

✅ Authentication Bypass Successful

✅ Patient Portal Access Obtained

---

## B.2 Patient Report Recovery

### Patient Report 1

| Item | Value |
|---------|---------|
| Status | Password Recovered |
| Recovered Password | 12345 |

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

## B.3 Validation Summary

| Asset | Access Obtained |
|---------|---------|
| Patient Report 1 | ✅ |
| Patient Report 2 | ✅ |
| Patient Report 3 | ✅ |
| Employee Records | ✅ |
| Shareholder Records | ✅ |
