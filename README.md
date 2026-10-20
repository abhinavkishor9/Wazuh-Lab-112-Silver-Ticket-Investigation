# Wazuh Lab 112 — Silver Ticket Investigation

## Overview

This lab explores how a SOC analyst can assess Windows authentication telemetry when investigating a potential Kerberos Silver Ticket attack. The investigation uses Windows Security logs, audit-policy settings, and Wazuh endpoint data to evaluate the available evidence and identify visibility limitations.

A Silver Ticket attack involves forging a Kerberos service ticket using a compromised service or computer account secret. The forged ticket may be presented directly to a target service, potentially without generating the normal service-ticket request event on a domain controller.

The objective of this lab is to assess investigation readiness using the available telemetry, not to simulate or claim detection of a confirmed attack.

## Objectives

- Understand the difference between Silver Ticket and Golden Ticket attacks.
- Review Windows Security events relevant to authentication and privileged logons.
- Examine audit-policy settings and their effect on event visibility.
- Validate available Windows Security telemetry in Wazuh.
- Review Wazuh Security Configuration Assessment (SCA) findings related to audit policy.
- Identify limitations caused by the absence of an Active Directory domain environment.
- Document findings and distinguish observed evidence from assumptions.

## Environment

| Component | Details |
|---|---|
| SIEM | Wazuh |
| Wazuh agent ID | `001` |
| Agent name | `DESKTOP-9MMM37V` |
| Agent IP | `192.168.203.1` |
| Endpoint OS | Windows 11 Pro |
| Endpoint configuration | Standalone workgroup workstation |
| Workgroup | `WORKGROUP` |
| Investigation focus | Kerberos service-ticket abuse and telemetry readiness |

## Investigation Scope

The investigation reviews the following Windows Security events:

| Event ID | Description | Relevance |
|---|---|---|
| 4624 | An account was successfully logged on | Establishes successful logon activity and available session details |
| 4625 | An account failed to log on | Provides context about unsuccessful authentication attempts |
| 4648 | A logon was attempted using explicit credentials | May help investigate activity involving supplied credentials |
| 4672 | Special privileges assigned to a new logon | Helps assess privileged logon sessions |
| 4769 | A Kerberos service ticket was requested | Relevant to service-ticket investigations when collected on a domain controller |

These events provide investigative context but are not individually conclusive evidence of a Silver Ticket attack.

## Investigation Findings

- The endpoint reports `WORKGROUP`, `PartOfDomain=False`, and `DomainRole=0`.
- The supplied local Security log output includes Events 4624 and 4672.
- The displayed Event 4672 record identifies the `SYSTEM` account under `NT AUTHORITY` and logon ID `0x3E7`.
- The checked `Logon` and `Special Logon` audit subcategories both reported `No Auditing`.
- Wazuh contains an SCA document recording a check of the `Special Logon` audit policy.
- No Active Directory domain controller is available for reviewing domain-side Kerberos service-ticket activity.

The available evidence does not establish that a Silver Ticket attack occurred.

## Investigation Limitations

The endpoint is not joined to an Active Directory domain and cannot provide domain-controller Security logs. A local workstation's authentication records are not a substitute for domain-controller and target-service telemetry.

A forged service ticket may be accepted by a target service without the usual domain-controller service-ticket request event. Therefore, the absence of Event 4769 alone does not confirm or rule out a Silver Ticket attack.

## Expected Outcome

The completed investigation should document the available authentication events, audit-policy findings, Wazuh telemetry, and environmental limitations. It should also identify the additional domain-controller and target-service evidence required for a more conclusive investigation.

**Conclusion:** This lab demonstrates evidence-based investigation readiness and telemetry assessment. It does not represent a confirmed Silver Ticket simulation or detection.
