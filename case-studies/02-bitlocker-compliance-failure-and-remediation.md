# BitLocker Compliance Failure and Remediation

> Status: Draft

## Scenario

A controlled BitLocker compliance failure was generated on an authorized Windows 11 lab device to evaluate how Microsoft Intune detects a loss of encryption protection.

After Intune identified the device as noncompliant, BitLocker was restored and the endpoint was validated from both the local operating system and the Intune management portal. A related security-baseline setting was then reviewed to evaluate how preventive controls could reduce the use of unencrypted fixed drives.

## Objectives

* Generate a controlled endpoint-encryption compliance failure.
* Observe how Intune reports the change in device posture.
* Evaluate the security risk created by missing encryption protection.
* Restore BitLocker and verify the remediation locally.
* Confirm that Intune recognized the corrected device state.
* Review a related security-baseline setting as a preventive control.
* Document the limitations of compliance-status propagation.

## Environment

* Microsoft Intune
* Microsoft Entra ID
* Windows 11 lab virtual machine
* BitLocker Drive Encryption
* Intune device-compliance policies
* Security Baseline for Windows 10 and later
* Windows `manage-bde` command-line utility

## Activities and Findings

### 1. Controlled Compliance Failure

BitLocker protection was disabled on the `win11-lab` virtual machine through the Windows BitLocker management interface.

After the device state was evaluated by Intune, the assigned compliance policy initially displayed an **Error** state. The BitLocker requirement was eventually reported as **Not compliant**.

This demonstrated that a change made locally on an endpoint can affect the device’s centrally reported compliance posture after the updated state reaches Intune.

### 2. Security-Risk Assessment

Disabling BitLocker reduced the protection applied to data stored on the device.

If an unencrypted or improperly protected endpoint were lost, stolen, or accessed outside normal operating controls, its stored information could be more exposed to unauthorized access. In a production environment, the noncompliant state could trigger investigation, remediation, or an access restriction through Conditional Access.

The result also demonstrated why a security team should investigate the specific failed control instead of responding only to the device’s overall compliance label.

### 3. BitLocker Remediation

BitLocker was restored on the operating-system drive. The following command was used to validate the local encryption state:

`manage-bde -status C:`

The command confirmed:

* Conversion status: Fully encrypted
* Percentage encrypted: 100%
* Protection status: Protection on

These results verified that encryption had been restored locally before relying on the management portal’s status.

### 4. Centralized Compliance Validation

After remediation and subsequent Intune evaluation, the device returned to a compliant state.

Validating the result in both locations provided two layers of confirmation:

* The local command verified the actual state of the operating-system drive.
* Intune verified that the centrally managed compliance state reflected the remediation.

This approach reduces the risk of treating a portal status as sufficient evidence when the underlying endpoint control has not been checked directly.

### 5. Security-Baseline Review

The Security Baseline for Windows 10 and later was reviewed for an additional BitLocker-related control.

The setting **Deny write access to fixed drives not protected by BitLocker** was disabled by default. This setting determines whether Windows prevents data from being written to fixed data drives that are not protected by BitLocker.

This setting applies to fixed data drives and is separate from the operating-system drive remediation performed earlier in the case study. In environments that prohibit the use of unencrypted fixed storage, enabling the setting could reduce the risk of data being written to an unprotected drive.

The setting should be tested before deployment because enforcement could interrupt legitimate workflows involving fixed drives that have not yet been encrypted.

## Compliance and Control Assessment

| Stage                 | Observation                                            | Security interpretation                                              |
| --------------------- | ------------------------------------------------------ | -------------------------------------------------------------------- |
| Control change        | BitLocker protection was disabled                      | The endpoint no longer satisfied the required encryption control     |
| Initial Intune result | Compliance policy reported an error                    | Intune had not yet produced a final compliance determination         |
| Compliance detection  | BitLocker was reported as not compliant                | The management platform identified the missing protection            |
| Risk                  | Stored data lacked the expected encryption protection  | Loss or unauthorized physical access could increase exposure         |
| Local remediation     | BitLocker was restored                                 | The required endpoint control was re-enabled                         |
| Local validation      | Drive was 100% encrypted with protection on            | The operating-system drive’s encryption state was directly confirmed |
| Intune validation     | Device returned to compliant                           | The centrally reported state reflected the remediation               |
| Preventive review     | Unprotected fixed-drive write restriction was disabled | A stricter configuration could reduce unencrypted-storage risk       |

## Security Considerations

Compliance-state changes may not appear immediately after an endpoint configuration changes. An interim **Error** or stale state should be investigated rather than interpreted as a final result.

Remediation should also be verified at the source. In this case, the local BitLocker status established that encryption was active before Intune confirmed the updated compliance state.

Security baselines provide recommended configurations, but each setting should be evaluated for scope and operational impact. The reviewed fixed-drive control should not be confused with the operating-system drive’s BitLocker status.

## Outcome

Microsoft Intune detected that the lab device no longer met its BitLocker requirement after encryption protection was disabled.

BitLocker was restored, the operating-system drive was verified as fully encrypted with protection enabled, and Intune later reported the device as compliant. The exercise demonstrated the complete lifecycle of a controlled compliance failure:

1. Security-control change
2. Centralized detection
3. Risk assessment
4. Endpoint remediation
5. Local verification
6. Centralized compliance confirmation
7. Preventive-control review

## Limitations

* The activity involved one authorized lab virtual machine.
* The compliance failure was generated intentionally and was not a real security incident.
* Intune required time to receive and evaluate the updated device state.
* Conditional Access enforcement was not tested during this activity.
* The reviewed fixed-drive baseline setting was assessed but not deployed.
* No production devices or policies were modified.

## Security Concepts Demonstrated

* Endpoint encryption
* BitLocker administration
* Data-at-rest protection
* Device compliance
* Configuration-drift detection
* Compliance-status propagation
* Endpoint remediation
* Local and centralized validation
* Conditional Access readiness
* Security-baseline assessment
* Operational-impact analysis

## Evidence

Supporting screenshots showing the BitLocker compliance failure, local remediation, restored Intune compliance, and reviewed security-baseline setting will be added next.
