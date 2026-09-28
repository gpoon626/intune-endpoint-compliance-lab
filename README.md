# Intune Endpoint Compliance Lab

> Repository Status: Complete

This repository documents hands-on Microsoft Intune case studies completed in an authorized Windows 11 lab environment. The project covers device enrollment, compliance investigation, BitLocker validation and remediation, Windows update posture, security-baseline review, and endpoint reporting.

## Case Studies

| Case study                                                                                                            | Status   |
| --------------------------------------------------------------------------------------------------------------------- | -------- |
| [Device Enrollment and Compliance Assessment](case-studies/01-device-enrollment-and-compliance-assessment.md)         | Complete |
| [BitLocker Compliance Failure and Remediation](case-studies/02-bitlocker-compliance-failure-and-remediation.md)       | Complete |
| [Windows Update Compliance and Patch Risk Investigation](case-studies/03-windows-update-compliance-and-patch-risk.md) | Complete |

## Key Findings

* A device’s aggregate compliance status does not explain which individual policy settings passed or failed.
* Local validation tools can confirm an endpoint’s actual security state before relying on centralized reporting.
* BitLocker compliance can be detected, investigated, remediated, and validated through both the endpoint and Intune.
* A generally compliant device can still be behind the target Windows security release.
* Current endpoint records must be distinguished from historical records when reviewing Intune reports.
* Device compliance and patch compliance should be monitored as separate security measurements.

## Technologies and Concepts

* Microsoft Intune
* Microsoft Entra ID
* Windows 11
* Azure virtual machines
* Device enrollment
* Endpoint compliance
* Compliance policies
* BitLocker
* Windows Update
* Windows Quality Update Status reporting
* Intune security baselines
* Patch-compliance analysis
* Endpoint reporting
* Root-cause investigation
* Security-control validation
* Local and centralized evidence correlation

## Repository Structure

```text
intune-endpoint-compliance-lab/
├── README.md
├── case-studies/
│   ├── 01-device-enrollment-and-compliance-assessment.md
│   ├── 02-bitlocker-compliance-failure-and-remediation.md
│   └── 03-windows-update-compliance-and-patch-risk.md
└── evidence/
    ├── device-enrollment-and-compliance/
    ├── bitlocker-compliance-remediation/
    └── windows-update-compliance/
```

## Scope and Ethics

* All activities were performed in an authorized lab environment.
* No production users, endpoints, or policies were modified.
* Controlled changes, including disabling BitLocker and temporarily pausing updates, were limited to the lab virtual machine.
* Security settings were restored after the required evidence was collected.
* Historical device records were identified and excluded from conclusions about the current endpoint.
* Findings are limited to the observed test environment and should not be generalized without additional validation.

