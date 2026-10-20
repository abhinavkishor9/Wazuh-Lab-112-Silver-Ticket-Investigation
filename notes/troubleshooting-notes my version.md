# Troubleshooting Notes 

## 1. Missing or Incomplete Kerberos Events

### Observation

The PowerShell query requested several Windows Security event IDs, including Event 4769. The supplied output excerpt shows Events 4624 and 4672.

### Assessment

The endpoint reports `WORKGROUP`, `PartOfDomain=False`, and `DomainRole=0`. It is not an Active Directory domain controller, and the environment has no available domain controller.

Event 4769 is particularly relevant to Kerberos service-ticket requests on domain controllers. Its absence from a standalone endpoint's reviewed output does not establish a detection failure or indicate an attack.

### Recommended handling

- Record which events are actually present in the supplied evidence.
- Avoid describing events as universally absent based on a partial output excerpt.
- Obtain domain-controller telemetry before attempting a domain-level Kerberos assessment.

## 2. Audit Policy Reports No Auditing

### Observation

The following subcategories both reported `No Auditing`:

- `Logon`
- `Special Logon`

### Assessment

These settings can limit the generation of relevant audit events. However, the presence of existing Security events does not necessarily contradict the current audit-policy output.

The available evidence does not establish whether earlier audit settings, policy changes, or other configuration conditions explain the observed records.

### Recommended handling

- Preserve the audit-policy output as evidence.
- Record when the policy was checked.
- If authorized and relevant to the lab, review effective policy configuration before making changes.
- Do not enable auditing merely to manufacture attack evidence.

## 3. Interpreting Event 4672

### Observation

Event 4672, record ID `2602`, identifies the SYSTEM account and logon ID `0x3E7`.

### Assessment

Privileged logon events can be generated during routine operating-system activity. The SYSTEM account commonly operates with extensive privileges.

Event 4672 alone does not establish unauthorized privilege escalation, ticket forgery, or a Silver Ticket attack.

### Recommended handling

- Examine the complete event message when available.
- Correlate the logon ID with relevant nearby events where possible.
- Assess the account, process, time, and system context before drawing conclusions.

## 4. Wazuh SCA Result Versus Attack Detection

### Observation

Wazuh contains an SCA document showing a check for:

`auditpol.exe /get /subcategory:"Special Logon"`

### Assessment

An SCA record evaluates or documents security configuration. It is not equivalent to a Windows authentication alert and does not establish that Kerberos ticket abuse was detected.

### Recommended handling

- Keep configuration findings separate from attack indicators.
- Record the exact SCA fields available in the document.
- Do not infer an SCA pass/fail result unless the corresponding result field is available.

## 5. Local Event Query Limitations

### Observation

The PowerShell query used `-ErrorAction SilentlyContinue` and limited its output to 100 events.

### Assessment

Silently suppressed errors can make troubleshooting less transparent. A result limited to 100 records may also omit events outside the returned set.

### Recommended handling

If a specific event requires further validation, use a narrowly scoped query and inspect the complete event message. Avoid repeating broad queries when the missing information cannot exist in the current environment.

