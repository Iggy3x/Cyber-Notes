# Windows / Active Directory Home Lab

Praktinis Windows enterprise / IT Support / Infrastructure home lab, pastatytas su Hyper-V, Windows Server, Active Directory, DNS, DHCP, Group Policy, LAPS ir PowerShell Remoting.

## Current architecture

| System | Role | Network |
|---|---|---|
| `COGITATOR` | Physical Windows 11 Hyper-V host | Host |
| `MM-DC01` | Windows Server 2025, AD DS, DNS, DHCP, Global Catalog | `10.10.10.10/24` static |
| `MM-WS01` | Windows 11 admin workstation, RSAT | `10.10.10.20/24` static + Hyper-V Default Switch for internet |
| `WS-02` | Windows 11 test workstation | DHCP, currently `10.10.10.101` |
| `LAB-Internal` | Hyper-V Internal Switch | `10.10.10.0/24`, no default gateway |

Domain: `ad.malunas.test`  
NetBIOS: `AD`

## Completed lab blocks

1. Hyper-V / VM / internal network foundation
2. Windows Server + AD DS + DNS
3. OUs, users, groups, domain join
4. SMB + NTFS permissions
5. Group Policy restrictions + mapped drives
6. RSAT admin workstation
7. Account lockout + Event Viewer
8. Windows LAPS
9. WinRM / PowerShell Remoting
10. Remote WS2 health check
11. Windows Time troubleshooting
12. DHCP Server deployment
13. WS2 static-to-DHCP migration
14. Reverse DNS / PTR

## Notes structure

- [`01-Foundation-AD-DNS-GPO.md`](01-Foundation-AD-DNS-GPO.md)
- [`02-RSAT-Admin-Workstation.md`](02-RSAT-Admin-Workstation.md)
- [`03-Account-Lockout-Event-Viewer.md`](03-Account-Lockout-Event-Viewer.md)
- [`04-LAPS.md`](04-LAPS.md)
- [`05-WinRM-PowerShell-Remoting.md`](05-WinRM-PowerShell-Remoting.md)
- [`06-Remote-Health-Check.md`](06-Remote-Health-Check.md)
- [`07-Time-Sync-Troubleshooting.md`](07-Time-Sync-Troubleshooting.md)
- [`08-DHCP-Server-Deployment.md`](08-DHCP-Server-Deployment.md)
- [`09-WS2-DHCP-Migration.md`](09-WS2-DHCP-Migration.md)
- [`10-Reverse-DNS-PTR.md`](10-Reverse-DNS-PTR.md)
- [`Command-Cheat-Sheet.md`](Command-Cheat-Sheet.md)
- [`Glossary.md`](Glossary.md)

## Troubleshooting workflow

`Symptom -> Hypothesis -> Test -> Evidence -> One change -> Verify`

Svarbiausia taisyklė iš labo: nekeisti kelių nesusijusių settingų vienu metu, nes tada prarandamas aiškus root cause.
