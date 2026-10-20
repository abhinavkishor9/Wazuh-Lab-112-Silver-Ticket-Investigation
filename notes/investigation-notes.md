# Investigation Notes — Lab 112: Silver Ticket Investigation

## 1. Investigation Summary

The investigation assessed whether the Windows endpoint monitored by Wazuh provided useful telemetry for reviewing a potential Kerberos Silver Ticket attack.

The available evidence included local Windows Security events, endpoint domain-membership information, audit-policy settings, and a Wazuh Security Configuration Assessment (SCA) document.

The investigation was limited to a standalone Windows workstation. No Active Directory domain controller was available for domain-level Kerberos analysis.

## 2. Environment Validation

The following endpoint details were observed:

| Property | Observed value |
|---|---|
| Computer name | `DESKTOP-9MMM37V` |
| Domain | `WORKGROUP` |
| PartOfDomain | `False` |
| DomainRole | `0` |
| Wazuh agent ID | `001` |
| Wazuh agent IP | `192.168.203.1` |

The results establish that the endpoint is a standalone workgroup computer rather than an Active Directory domain member.

This distinction limits the Kerberos evidence available for the investigation.

## 3. Windows Security Event Review

The following events appeared in the supplied PowerShell output:

### Event ID 4672 — Special Privileges Assigned to a New Logon

- Timestamp: `19-10-2026 07:37:13`
- Record ID: `2602`
- Provider: `Microsoft-Windows-Security-Auditing`
- Security ID: `S-1-5-18`
- Account: `SYSTEM`
- Account domain: `NT AUTHORITY`
- Logon ID: `0x3E7`

The event lists privileges including `SeDebugPrivilege`, `SeBackupPrivilege`, `SeRestorePrivilege`, and `SeImpersonatePrivilege`.

These privileges are associated with the SYSTEM security context. The event does not, by itself, indicate malicious activity or Kerberos ticket forgery.

### Event ID 4624 — Successful Logon

The supplied command output also includes Event ID 4624 with record ID `2601`, at `19-10-2026 07:37:13`.

The available excerpt does not include the event's complete message. Therefore, the account, logon type, source address, and authentication details cannot be reliably documented from this evidence alone.

### Other Events Reviewed

The PowerShell query requested Events 4624, 4625, 4648, 4672, and 4769 over the previous seven days. The supplied output excerpt displays Events 4624 and 4672.

The excerpt does not establish the complete availability or absence of every requested event. Event 4769 should be assessed in the context of the endpoint's workgroup configuration and the lack of domain-controller telemetry.

## 4. Audit Policy Review

The following commands were executed:

```powershell
auditpol /get /subcategory:"Logon"
auditpol /get /subcategory:"Special Logon"
```

Both subcategories reported:

- Logon: `No Auditing`
- Special Logon: `No Auditing`

These results describe the audit-policy settings observed during the check. They do not establish that all previously recorded events must be absent.

Existing records may have been generated under different earlier settings or policy conditions. The precise cause cannot be established from the supplied output alone.

## 5. Wazuh SCA Evidence

A Wazuh Discover document contained the following fields:

| Field | Observed value |
|---|---|
| Index | `wazuh-alerts-4.x-2026.10.20` |
| Agent ID | `001` |
| Agent name | `DESKTOP-9MMM37V` |
| Check command | `auditpol.exe /get /subcategory:"Special Logon"` |
| CIS reference | `17.5.6` |

Additional compliance references were displayed for CIS Controls v7 and v8, CMMC, and ISO 27001.

This document demonstrates that Wazuh recorded an SCA check related to the Special Logon audit policy. It is configuration-assessment evidence, not proof of a Silver Ticket attack or a successful attack detection.

## 6. Findings and Assessment

### Confirmed observations

- The endpoint is configured as a standalone workgroup computer.
- Events 4624 and 4672 appear in the supplied local Security log output.
- The displayed Event 4672 record belongs to the SYSTEM account.
- Both checked audit subcategories reported `No Auditing`.
- Wazuh contains an SCA document for the Special Logon audit-policy check.
- The investigation environment has no available Active Directory domain controller.

### Unresolved questions

- What are the complete details of Event 4624, including its logon type and authentication information?
- What audit-policy settings applied when the displayed events were generated?
- What additional telemetry would be available from a target service and domain controller?
- Are there any independently corroborated indicators of suspicious service-ticket use? The current evidence does not establish this.

## 7. Conclusion

The available evidence supports a review of endpoint authentication telemetry and audit-policy visibility. It does not establish a Silver Ticket attack.

A stronger investigation would require appropriate Active Directory domain-controller logs, target-service authentication records, relevant endpoint telemetry, and correlation of any suspicious service access with other supporting evidence.
