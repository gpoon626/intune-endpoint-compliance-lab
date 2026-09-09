# Device Enrollment and Compliance Assessment

> Status: Draft

## Scenario

A Windows 11 lab virtual machine was reviewed in Microsoft Intune to verify successful enrollment and evaluate its compliance status. Although the device was enrolled and compliant with its assigned security policy, its overall status was reported as noncompliant because a setting in the Default Device Compliance Policy failed.

## Objectives

* Verify that the Windows 11 lab device was enrolled in Intune.
* Review the device’s overall compliance status.
* Examine compliance at the individual policy and setting levels.
* Distinguish assigned-policy compliance from aggregate device compliance.
* Evaluate how the device state could affect Conditional Access.
* Identify an appropriate follow-up investigation without weakening security controls.

## Environment

* Microsoft Intune
* Microsoft Entra ID
* Windows 11 lab virtual machine
* Intune device compliance
* Windows Defender Threat Compliance policy
* Default Device Compliance Policy

## Activities and Findings

### 1. Device-Enrollment Verification

The `win11-lab` virtual machine was confirmed to be enrolled and visible in Microsoft Intune.

The device record demonstrated that Intune could centrally identify the managed endpoint and make it available for policy, compliance, configuration, and application management.

### 2. Compliance-Status Review

The device’s overall compliance status was reported as **Noncompliant**. The individual policy results provided additional context:

* **Windows Defender Threat Compliance:** Compliant
* **Default Device Compliance Policy:** Noncompliant
* **Noncompliant setting:** `Is active`

This distinction was important because the overall compliance label did not mean that every evaluated security control had failed. The device passed its assigned Windows Defender Threat Compliance policy, while one setting in the default policy caused the aggregate device status to become noncompliant.

### 3. Preliminary Cause Assessment

Because `win11-lab` is a lab virtual machine that is regularly shut down, the `Is active` result may be related to device activity or check-in recency.

However, the available evidence did not conclusively establish the cause. A follow-up investigation should review the device’s last check-in information, activity history, and subsequent compliance evaluations before identifying inactivity as the confirmed root cause.

### 4. Conditional Access Significance

Microsoft Entra Conditional Access can use an Intune device’s compliance state when determining whether to grant access.

If a policy requires a compliant device, an overall **Noncompliant** result could prevent access even when the device passes an assigned security policy. This makes it important to examine the individual policy and setting results rather than relying exclusively on the aggregate status.

## Compliance Assessment

| Evidence or control       | Observation                                      | Interpretation                                                                  |
| ------------------------- | ------------------------------------------------ | ------------------------------------------------------------------------------- |
| Device enrollment         | `win11-lab` appeared in Intune                   | The endpoint was successfully enrolled and available for centralized management |
| Overall compliance        | Noncompliant                                     | At least one evaluated requirement was not satisfied                            |
| Assigned security policy  | Windows Defender Threat Compliance was compliant | The device passed the evaluated Defender requirements                           |
| Default compliance policy | Noncompliant                                     | A default-policy setting affected the aggregate result                          |
| Failed setting            | `Is active`                                      | The device did not satisfy the activity-related evaluation                      |
| Preliminary cause         | The lab VM is regularly shut down                | Inactivity or check-in recency is a reasonable hypothesis requiring validation  |
| Conditional Access impact | Compliance can be used as a grant requirement    | An overall noncompliant result could affect access decisions                    |

## Security Considerations

An aggregate compliance result should be investigated at the individual policy and setting levels. A device may satisfy several security requirements while still being marked noncompliant because of one failed evaluation.

Compliance data must also be current and accurate when it supports Conditional Access decisions. An outdated or inactive device record could result in an access decision that does not reflect the endpoint’s present security configuration.

The investigation should determine why the setting failed before changing the policy or device. Security controls should not be disabled solely to reproduce a noncompliant state when an existing condition can be investigated safely.

## Outcome

The review confirmed that `win11-lab` was successfully enrolled in Microsoft Intune. It also identified a difference between the device’s assigned-policy result and its overall compliance state.

The device complied with the Windows Defender Threat Compliance policy but remained noncompliant because `Is active` failed under the Default Device Compliance Policy.

The exact cause was not confirmed during this activity. The result established a clear follow-up investigation involving device activity, check-in status, and compliance reevaluation.

## Limitations

* The assessment involved one authorized lab virtual machine.
* The device’s historical check-in information was not fully investigated.
* The cause of the `Is active` failure remains a hypothesis.
* Conditional Access enforcement was not tested during this activity.
* No security controls were weakened to manufacture a failure.
* No production endpoints or policies were modified.

## Security Concepts Demonstrated

* Endpoint enrollment
* Centralized device management
* Device compliance
* Policy-level analysis
* Aggregate compliance evaluation
* Conditional Access
* Evidence-based troubleshooting
* Root-cause validation
* Least-disruptive investigation
* Security-control preservation

## Evidence

Supporting screenshots showing the enrolled device and its policy-level compliance results will be added next.
