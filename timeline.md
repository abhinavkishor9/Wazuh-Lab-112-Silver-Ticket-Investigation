# Timeline — Lab 112: Silver Ticket Investigation

## Purpose

This timeline records the observed timestamps and investigation activities supported by the supplied evidence. It distinguishes event timestamps from the time the investigation workspace was created.

| Date and time | Evidence or activity | Observation |
|---|---|---|
| 19 Oct 2026, 07:37:13 | Windows Security Event 4672, record ID `2602` | Special privileges assigned to the SYSTEM account; logon ID `0x3E7`. |
| 19 Oct 2026, 07:37:13 | Windows Security Event 4624, record ID `2601` | Successful logon event appears in the supplied output. The complete event message was not provided. |
| 20 Oct 2026, 07:00 | Investigation workspace creation | Created `C:\WazuhSilverTicketLab112\Evidence` and `C:\WazuhSilverTicketLab112\Notes`. |
| During investigation | Endpoint configuration validation | Confirmed `WORKGROUP`, `PartOfDomain=False`, and `DomainRole=0`. |
| During investigation | Audit-policy review | `Logon` and `Special Logon` both reported `No Auditing`. |
| During investigation | Wazuh SCA review | Located a document recording the Special Logon audit-policy check for agent `001`. |

## Interpretation

The supplied Security events establish that successful logon and privileged-logon records were present in the reviewed output. The displayed Event 4672 identifies the SYSTEM account; it does not establish malicious activity.

The audit-policy results identify a configuration visibility concern. The Wazuh SCA document provides evidence of a configuration check, not evidence of a Silver Ticket attack.

## Timeline Limitations

- The timestamps above reflect the supplied screenshots and command output.
- No exact timestamp was supplied for the endpoint configuration check or Wazuh SCA review.
- The full Event 4624 message was not available in the supplied excerpt.
- No domain-controller event timeline was available.
- No confirmed Silver Ticket activity was established.

## Conclusion

The timeline documents endpoint evidence collection and the investigation's environmental constraints. Further domain-controller and target-service evidence would be required to build a timeline for a suspected Silver Ticket attack.
