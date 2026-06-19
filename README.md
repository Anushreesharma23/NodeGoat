# Checkmarx One CI/CD Security Automation

## Project Overview

This project demonstrates the integration of Checkmarx One security scanning into a Continuous Integration (CI) pipeline using Python and GitHub Actions.

The solution automatically performs source code security analysis, generates vulnerability reports, summarizes findings, and sends email notifications to stakeholders whenever a commit happens on the repository. It also includes a pre-flight validation utility to detect configuration issues before initiating scans.

---

## Features

### Automated Security Scanning

* Integrates with Checkmarx One CLI (`cx`)
* Performs SAST security scans on source code
* Generates JSON vulnerability reports

### Pre-Flight Validation

* Validates required configuration before scanning
* Verifies Checkmarx authentication
* Checks SMTP connectivity
* Ensures source paths are accessible
* Fails fast to save CI execution time

### Email Reporting

* Generates HTML security summaries
* Reports findings by severity level
* Highlights top vulnerabilities
* Supports multiple recipients

### CI/CD Integration

* GitHub Actions workflow included
* Automated execution on push and manual trigger
* Uploads scan reports as workflow artifacts

### Cross-Platform Support

* Ubuntu
* Windows
* macOS

---

## Repository Structure

```
.
├── checkmarx_scan.py
├── validate_config.py
├── requirements.txt
├── config.example.json
├── .gitignore
├── ReadMe.txt
└── .github/
    └── workflows/
        └── checkmarx-scan.yml
```

---

## Prerequisites

* Python 3.7+
* Checkmarx One CLI (`cx`)
* Checkmarx One Account
* API Key
* Tenant Name
* SMTP Email Credentials

---

## Installation

### Install Checkmarx CLI

Download and install the latest Checkmarx CLI from the official releases page.

Verify installation:

```bash
cx version
```

### Configure Settings

Copy the sample configuration:

```bash
cp config.example.json config.json
```

Update the file with:

* Checkmarx API Key
* Tenant Name
* SMTP Credentials
* Recipient Emails

---

## Running Configuration Validation

Before starting a scan:

```bash
python validate_config.py
```

The validator checks:

1. Required configuration values
2. CLI availability
3. Checkmarx authentication
4. SMTP connectivity
5. Email formatting
6. Source path accessibility

---

## Running a Security Scan

```bash
python checkmarx_scan.py
```

Optional parameters:

```bash
python checkmarx_scan.py --skip-email
python checkmarx_scan.py --config custom.json
```

---

## GitHub Actions Setup

Add the following repository secrets:

| Secret           | Description          |
| ---------------- | -------------------- |
| CX_BASE_URI      | Checkmarx Server URL |
| CX_TENANT        | Tenant Name          |
| CX_APIKEY        | API Key              |
| CX_GROUP         | Group Name           |
| SMTP_SERVER      | SMTP Host            |
| SMTP_PORT        | SMTP Port            |
| SMTP_USER        | Email Username       |
| SMTP_PASSWORD    | Email Password       |
| EMAIL_RECIPIENTS | Recipient List       |

After configuration:

1. Push code to GitHub
2. Open Actions tab
3. Run "Checkmarx One Security Scan"

---

## Scan Workflow

The pipeline performs the following steps:

1. Checkout source code
2. Install Python dependencies
3. Install Checkmarx CLI
4. Validate configuration
5. Execute security scan
6. Generate JSON report
7. Analyze vulnerabilities
8. Send email summary
9. Upload report artifacts
10. Fail build on Critical or High findings

---

## Build Failure Policy

By default, the pipeline fails when:

* Critical vulnerabilities are detected
* High severity vulnerabilities are detected

This behavior can be customized using:

```text
FAIL_ON_SEVERITY=critical,high
```

---

## Email Summary

The generated email contains:

* Critical findings count
* High findings count
* Medium findings count
* Low findings count
* Informational findings count
* Top detected vulnerability categories

Example subject:

```text
[Checkmarx] Security Scan - C:2 H:5 M:12 L:8
```

---

## Cross-Platform Execution

The GitHub Actions workflow runs on:

* Ubuntu
* Windows
* macOS

To prevent duplicate notifications, only the Ubuntu job sends email while all operating systems execute the complete scan process.

---

## Troubleshooting

### Checkmarx CLI Not Found

```bash
cx version
```

Ensure the CLI is installed and available in PATH.

### Authentication Failure

Verify:

* CX_APIKEY
* CX_TENANT
* CX_BASE_URI

### SMTP Authentication Error

Use an App Password rather than your regular email password.

### Missing Report File

Verify that the configured output directory exists and is writable.

---

## Technologies Used

* Python
* Checkmarx One CLI
* GitHub Actions
* SMTP Email
* JSON Reporting

---

## Author

Anushree Sharma

Security Automation Project using Checkmarx One CI/CD Integration.
