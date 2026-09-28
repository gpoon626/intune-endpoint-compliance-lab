# Windows Update Compliance and Patch Risk Investigation

> Status: Complete

## Scenario

An Intune-managed Windows 11 lab device appeared compliant in the general device-compliance report. A separate review found that no update ring was assigned and the device was behind the current Windows security release.

The investigation compared local operating-system information, Windows Update history, the Intune Windows Quality Update Status report, and the organization-wide device-compliance report.

## Objectives

* Review the endpoint’s update-management configuration
* Determine the installed Windows build
* Compare the installed and target security releases
* Identify missing quality updates
* Distinguish device compliance from patch compliance
* Recommend improvements to patch monitoring

## Environment

* Authorized Microsoft 365 lab tenant
* Microsoft Intune
* Windows 11 virtual machine named `win11-lab`
* Windows 11 version 25H2
* Current lab build: `10.0.26200.9168`
* Windows Update
* Windows Quality Update Status report
* Device Compliance report

## Activities and Findings

### 1. Update-Management Configuration Review

The Windows update-ring configuration was reviewed in Intune. No update rings were configured or assigned to `win11-lab`.

Because no update ring was assigned, Intune was not centrally enforcing settings such as:

* Quality-update deferral periods
* Installation deadlines
* Grace periods
* Restart behavior

This created a management gap because the endpoint could receive updates without a defined organizational deployment schedule.

### 2. Local Patch-Level Validation

Windows Update history did not show an installed quality update. The visible history contained driver updates and Microsoft Defender definition updates.

The `winver` command confirmed that the device was running:

* Windows 11 version 25H2
* OS build `26200.9168`

At the time of the investigation, Microsoft’s release information identified build `26200.9457` as the current Windows 11 25H2 build. This confirmed that the endpoint was behind the current patch level.

### 3. Missing Update Detection

Updates were temporarily paused on the lab device to evaluate how Intune represented an outdated endpoint. After the device synchronized with Intune, the Windows Quality Update Status report listed `win11-lab` as **Not up to date**.

The report showed:

* Installed release: `2026.08 B Security`
* Target release: `2026.09 B Security`
* Update-ring assignment: None

The report displayed two records named `win11-lab`. The record on build `10.0.26200.9168` matched the local `winver` result and represented the current lab device. The record on build `10.0.26200.8246` came from an earlier VM that had been deleted and did not represent the current endpoint.

Using the current build record, Intune showed that the device had `2026.08 B Security` installed while `2026.09 B Security` was the target release. This established that the current device was one monthly security release behind. Windows Updates were resumed after the evidence was collected.

### 4. Organization-Wide Compliance Comparison

The Intune Device Compliance report was generated and exported. This report provided an organization-wide view of device-compliance records in the tenant.

The report showed:

- 17 device records
- 1 compliant record
- 16 noncompliant records
- The current `win11-lab` record on build `10.0.26200.9168` marked as compliant
- A historical `win11-lab` record on build `10.0.26200.8246` marked as noncompliant

The older record belonged to a previous lab VM that had been deleted. It was not used to determine the current endpoint’s state. The record on build `10.0.26200.9168` matched the local `winver` result and was treated as the current device.

The current device was compliant with its assigned device policies while the Windows Quality Update Status report showed that it was not up to date. This demonstrated that general device compliance and patch compliance measure different parts of endpoint security.

## Assessment

| Area reviewed                | Finding                 | Security significance                                                |
| ---------------------------- | ----------------------- | -------------------------------------------------------------------- |
| Update-ring assignment       | No update ring assigned | Update deadlines and grace periods were not centrally controlled     |
| Local operating-system build | Build `26200.9168`      | The installed build was behind the current release                   |
| Quality-update status        | Not up to date          | The device was missing the target monthly security release           |
| Installed security release   | `2026.08 B Security`    | The endpoint was one release behind                                  |
| Target security release      | `2026.09 B Security`    | An update was required to reach the target patch level               |
| General device compliance | Current build compliant; historical build noncompliant | General compliance was separate from the Windows quality-update status |

## Security Considerations

A compliant device is not necessarily current on security updates. Device-compliance policies only evaluate the settings included in those policies.

Patch posture should be monitored separately because delayed quality updates may leave endpoints exposed to vulnerabilities that have already been addressed by Microsoft.

Recommended improvements include:

* Configure and assign an Intune update ring
* Define appropriate quality-update deferrals, deadlines, and grace periods
* Review Windows Quality Update Status reports regularly
* Investigate devices that fall behind the target security release
* Compare centralized reporting with local build information when results are unclear
* Track device compliance and patch compliance as separate security measurements

## Outcome

The investigation confirmed that `win11-lab` was behind the target Windows security release even though Intune reported the device as generally compliant.

Local operating-system information and the Windows Quality Update Status report provided the evidence needed to identify the patch gap. Updates were resumed after testing.

The primary finding was that organizations should not rely on a general device-compliance result as proof that an endpoint is fully patched.

## Limitations

* The investigation involved one authorized lab virtual machine.
- The organization-wide report contained records for devices outside the endpoint being investigated.
- Intune displayed a historical record for an earlier deleted `win11-lab` VM.
- The investigation used build `10.0.26200.9168` because it matched the local `winver` result.
* No update ring was assigned during the investigation.
* Update-ring settings were reviewed conceptually because no existing policy was available.
* The findings represent the device’s patch state at the time the evidence was collected.
* No production systems or user devices were affected.

## Skills Demonstrated

* Microsoft Intune administration
* Windows update-status validation
* Patch-compliance analysis
* Endpoint risk assessment
* Operating-system build verification
* Security-report interpretation
* Multi-source evidence correlation
* Remediation planning

## Evidence

| Evidence | What it demonstrates |
|---|---|
| [No update rings configured](../evidence/windows-update-compliance/01-intune-no-update-rings-configured.png) | No Intune update-ring policies were configured |
| [Windows Update history](../evidence/windows-update-compliance/02-windows-update-history.png) | The visible history contained driver and Defender definition updates but no quality update |
| [Current Windows build](../evidence/windows-update-compliance/03-windows-build-version.png) | The current VM was running Windows 11 25H2, build `26200.9168` |
| [Paused updates and pending security update](../evidence/windows-update-compliance/04-paused-updates-and-pending-security-update.png) | The procedural update pause and pending September 2026 security update |
| [Intune quality-update status](../evidence/windows-update-compliance/05-intune-quality-update-status.png) | The current build was not up to date and was one monthly security release behind |
| [Organization-wide device compliance](../evidence/windows-update-compliance/06-organization-wide-device-compliance.png) | The current VM was generally compliant even though it was behind the target security release |

Additional descriptions and full-size screenshots are available in the [evidence documentation](../evidence/windows-update-compliance/README.md).
