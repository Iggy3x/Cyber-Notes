# 02 — RSAT / Admin Workstation

## Goal

Move routine domain administration away from the Domain Controller desktop and use `MM-WS01` as the administration workstation.

Model:

`Admin account -> MM-WS01 -> RSAT -> MM-DC01 / AD / GPO / DNS`

## Installed / verified

- Active Directory Users and Computers (`dsa.msc`)
- Group Policy Management
- DNS Manager

## Troubleshooting

### Admin group membership did not appear immediately

`whoami /groups` did not initially show the expected effective groups after AD membership changes.

Important correction:
- setting `Domain Admins` as the Primary Group was not required;
- the relevant issue was the existing Windows logon token;
- a fresh sign-in session allowed the updated group membership to appear.

### RSAT capability check failed

`Get-WindowsCapability -Name RSAT* -Online`

Initially failed because PowerShell was not elevated.

After running elevated PowerShell, required RSAT components showed `NotPresent`.

The components were installed through Windows Optional Features.

## Lessons

- RSAT installs management tools; it does not grant permissions.
- Admin rights come from account privileges / groups / delegation.
- AD group membership changes may require a new logon token.
- "Requires elevation" is a privilege problem, not necessarily a command typo.
