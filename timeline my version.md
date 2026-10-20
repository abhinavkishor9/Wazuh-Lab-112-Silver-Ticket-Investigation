# Timeline 
| Date and time | Evidence or activity | Observation |
|---|---|---|
| 19 Oct 2026, 07:37:13 | Windows Security Event 4672, record ID `2602` | Special privileges assigned to the SYSTEM account; logon ID `0x3E7`. |
| 19 Oct 2026, 07:37:13 | Windows Security Event 4624, record ID `2601` | Successful logon event appears in the supplied output. The complete event message was not provided. |
| 20 Oct 2026, 07:00 | Investigation workspace creation | Created `C:\WazuhSilverTicketLab112\Evidence` and `C:\WazuhSilverTicketLab112\Notes`. |
| During investigation | Endpoint configuration validation | Confirmed `WORKGROUP`, `PartOfDomain=False`, and `DomainRole=0`. |
| During investigation | Audit-policy review | `Logon` and `Special Logon` both reported `No Auditing`. |
| During investigation | Wazuh SCA review | Located a document recording the Special Logon audit-policy check for agent `001`. |

