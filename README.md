# Intune Endpoint Compliance Lab

> Repository Status: Active

This repository documents hands-on Microsoft Intune case studies completed in an authorized lab environment. The project focuses on endpoint enrollment, device compliance, policy-level investigation, security-control validation, and the relationship between Intune compliance and Microsoft Entra Conditional Access.

## Case Studies

| Case study | Tickets | Status |
|---|---|---|
| [Device Enrollment and Compliance Assessment](case-studies/01-device-enrollment-and-compliance-assessment.md) | CA-0017 and CA-0018 | Complete |

## Current Case Study

### Device Enrollment and Compliance Assessment

A Windows 11 lab virtual machine was verified as successfully enrolled and managed through Microsoft Intune.

The device passed its assigned Windows Defender Threat Compliance policy but remained noncompliant because `Is active` failed under the Default Device Compliance Policy. The assessment demonstrates why administrators must examine individual policy and setting results instead of relying only on a device’s aggregate compliance label.

[Read the complete case study](case-studies/01-device-enrollment-and-compliance-assessment.md)

## Planned Case Studies

Future case studies may cover:

- Noncompliant-device investigation and remediation
- BitLocker and endpoint security validation
- Security-baseline assessment
- Windows update and patch-compliance investigation
- Compliance reporting and operational monitoring

Planned topics will be added only after the corresponding lab activities and evidence have been completed.

## Technologies and Concepts

- Microsoft Intune
- Microsoft Entra ID
- Windows 11
- Azure virtual machines
- Device enrollment
- Endpoint compliance
- Compliance policies
- Conditional Access
- Root-cause investigation
- Security-control validation

## Repository Structure

```text
intune-endpoint-compliance-lab/
├── README.md
├── case-studies/
│   └── 01-device-enrollment-and-compliance-assessment.md
└── evidence/
    └── device-enrollment-and-compliance/
        ├── README.md
        ├── 01-intune-device-enrollment-status.png
        ├── 02-device-compliance-policy-results.png
        └── 03-default-policy-is-active-failure.png
