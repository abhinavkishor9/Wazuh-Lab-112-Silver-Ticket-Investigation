# Wazuh-Lab-112-Silver-Ticket-Investigation
## Overview
A Silver Ticket attack involves forging a Kerberos service ticket (TGS) using a compromised service account secret or computer account secret. Unlike a Golden Ticket attack, which targets the krbtgt account to forge TGTs, a Silver Ticket targets a specific service and may allow an attacker to access that service without a normal ticket request to a domain controller.

This lab explores how a SOC analyst can assess Windows authentication telemetry when investigating a potential Kerberos Silver Ticket attack. The investigation uses Windows Security logs, audit-policy settings, and Wazuh endpoint data to evaluate the available evidence and identify visibility limitations.

A Silver Ticket attack involves forging a Kerberos service ticket using a compromised service or computer account secret. The forged ticket may be presented directly to a target service, potentially without generating the normal service-ticket request event on a domain controller.

The objective of this lab is to assess investigation readiness using the available telemetry, not to simulate or claim detection of a confirmed attack.

## Lab Objectives

- Understand the mechanics of Kerberos Silver Ticket attacks and how they differ from Golden Ticket attacks.
- Identify Windows Security events relevant to investigating authentication activity, including Event IDs 4624, 4625, 4648, 4672, and 4769.
- Examine available endpoint authentication records and identify the details needed to assess potentially suspicious service access.
- Review Windows audit-policy settings and understand how auditing configuration can affect security event visibility.
- Validate Wazuh telemetry and examine Security Configuration Assessment (SCA) findings related to Windows audit policies.
- Correlate authentication events with account context, logon identifiers, timestamps, and other available evidence.
- Distinguish routine privileged system activity from indicators that may warrant further investigation.
- Document telemetry gaps and explain the limitations of investigating Kerberos service-ticket abuse without an Active Directory domain controller.
- Identify the additional domain-controller and target-service evidence required to investigate suspected Silver Ticket activity more conclusively.
- Produce evidence-based investigation notes that separate confirmed observations, unresolved questions, and investigative limitations.

## Lab Scenario

A SOC analyst is reviewing Windows endpoint telemetry collected by Wazuh following a hypothetical report of suspicious access to an internal service. The investigation focuses on assessing whether the available authentication records provide useful evidence for examining a potential **Kerberos Silver Ticket attack**.

A Silver Ticket attack involves forging a Kerberos service ticket using a compromised service account or computer account secret. An attacker may use the forged ticket to access a specific service without generating the usual service-ticket request event on a domain controller. Consequently, investigating this technique requires careful analysis of target-service activity and supporting authentication evidence.

During the investigation, the analyst reviews Windows Security events, checks audit-policy settings, and examines Wazuh's Security Configuration Assessment (SCA) data. Events 4624 and 4672 are present in the supplied local logs, while the `Logon` and `Special Logon` audit subcategories report `No Auditing`.

The endpoint is configured as a standalone workgroup computer rather than an Active Directory domain member. As a result, the available evidence cannot provide the domain-controller perspective needed to investigate Kerberos service-ticket activity comprehensively.

### Investigation Focus

- Examine available successful logon and privileged-logon records.
- Review audit-policy settings and assess their potential impact on event visibility.
- Validate Wazuh's available endpoint telemetry and configuration-assessment evidence.
- Distinguish routine system activity from behavior that would require further investigation.
- Identify the evidence gaps caused by the absence of an Active Directory domain controller and target-service logs.

**Expected outcome:** An evidence-based investigation record documenting observed authentication activity, audit-policy findings, telemetry limitations, and the additional evidence required to assess suspected Silver Ticket activity. The lab evaluates investigation readiness; it does not claim that a Silver Ticket attack was executed, confirmed, or detected.

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

