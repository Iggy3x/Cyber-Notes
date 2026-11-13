# 01 — Foundation: Hyper-V, AD, DNS, SMB, NTFS, GPO

## Infrastructure

- Host: `COGITATOR`, Windows 11 + Hyper-V
- Internal switch: `LAB-Internal`
- Network: `10.10.10.0/24`
- DC: `MM-DC01`, `10.10.10.10`
- Domain: `ad.malunas.test`
- DNS: `MM-DC01`
- Workstation: `MM-WS01`, `10.10.10.20`

## Active Directory

Configured:
- AD DS
- DNS Server
- Global Catalog
- department OUs
- domain test users
- Global Security Groups
- workstation domain join

Core model:

`User -> Security Group -> Resource Permission`

Important:
- OU != Security Group
- OU organizes objects and receives GPO links
- Groups are used for permissions / role-based access

## Networking lessons

- DC and workstation must have unique IPs.
- Domain workstation DNS points to the AD DNS server: `10.10.10.10`.
- Public DNS such as `8.8.8.8` is not the correct primary DNS for an AD domain member.
- `10.10.10.0/24` uses subnet mask `255.255.255.0`.

An early IP conflict occurred when DC and WS1 used the same address. It was fixed by keeping:
- DC: `10.10.10.10`
- WS1: `10.10.10.20`

## SMB + NTFS

Physical folders were created under `C:\Shares`.

Example network shares:
- `\\MM-DC01\IT-Shares`
- `\\MM-DC01\Finances`

Access model:

`User -> SMB Share Permission -> NTFS Permission -> File/Folder`

Lessons:
- Creating a "Shared Folder" object in ADUC does not create a physical SMB share.
- Broad `Everyone` permissions caused unintended access and were removed.
- Sharing the parent `C:\Shares` created multiple network paths and confusing effective permissions; the parent share was removed.
- Permissions were assigned to department groups rather than individual users.

## Group Policy

Practiced:
- GPO creation
- linking GPO to OU
- User Configuration vs Computer Configuration
- Policies vs Preferences
- mapped drives
- workstation restrictions
- `gpupdate /force`

Example mapped drive:
- `F:` -> `\\MM-DC01\Finances`

One mapped-drive failure was caused by a UNC typo:
- wrong: `\\MM-DC01\Finance`
- correct: `\\MM-DC01\Finances`

Lesson: verify exact names and paths before changing larger configuration.
