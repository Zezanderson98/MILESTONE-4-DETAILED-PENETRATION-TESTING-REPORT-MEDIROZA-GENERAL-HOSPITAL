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
- SS003-NSLOOKUP.png
- SS006-DNSRECON.png

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
- SS004-CURL-I.png

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

- SS016-OLD-DIRECTORY.png
- SS017-DATABASE-BACKUP.png
- OLD PAGE.png

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

- SS014-PATIENT-LOGIN.png
- SS018-PATIENT-PORTAL.png
- PDF-PATIENT-DISCOVERED.png

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

### Evidence

- PDF-01.png
- PDF-02.png
- PDF-03.png

### Impact

Successful password recovery allowed unauthorized access to confidential patient medical information.

### Root Cause

- Weak password selection
- Predictable passwords
- Lack of password complexity requirements

---

# Finding F-008

## Employee Salary Information Exposure

### Severity

**CRITICAL**

### Description

The exposed SQL backup contained sensitive payroll and employment information belonging to hospital personnel.

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
### Evidence

- EMP-01.png

### Impact

Exposure of employee salary information may result in:

- Privacy violations
- Targeted social engineering
- Employee disputes
- Financial profiling
- Reputational damage

---

# Finding F-009

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
| F-007 | Weak Password Protection on Patient Reports | High |
| F-008 | Employee Salary Information Exposure | Critical |
| F-009 | Shareholder Ownership Information Exposure | Critical |

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

## A.3 Validation Summary

| Asset | Access Obtained |
|---------|---------|
| Patient Report 1 | ✅ |
| Patient Report 2 | ✅ |
| Patient Report 3 | ✅ |
| Employee Records | ✅ |
| Shareholder Records | ✅ |



