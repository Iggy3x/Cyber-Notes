# 04 — Windows LAPS

## Goal

Manage a local administrator password on `WS-02` centrally through Windows LAPS and Active Directory.

Managed local account:
- `WLapsAdmin`

Local login formats:
- `.\WLapsAdmin`
- `WS-02\WLapsAdmin`

## Key configuration

LAPS policy was configured under Computer Configuration.

A major GPO scope problem was identified:
- Security Filtering != GPO Link
- the GPO existed but initially was not linked where WS2 could receive it

`WS-02` was moved into a proper `Workstations` OU because the default `Computers` object is a container, not an OU.

Verification:
- `gpresult /scope computer /r`
- LAPS appeared under Applied Group Policy Objects

## AD schema issue

LAPS processing reported that Active Directory did not contain the required Windows LAPS schema.

The schema was extended:

`Update-LapsADSchema`

Then workstation computer objects were granted permission to write their LAPS attributes:

`Set-LapsADComputerSelfPermission -Identity "OU=Workstations,DC=ad,DC=malunas,DC=test"`

## Verification

- ADUC displayed the LAPS password for WS2
- local login with `WLapsAdmin` succeeded
- password rotation was tested
- `Invoke-LapsPolicyProcessing` and `Reset-LapsPassword` were used during validation

## Lessons

- LAPS manages local admin passwords, not normal domain-user passwords.
- GPO link and Security Filtering solve different problems.
- `Computers` container != OU.
- Schema support and AD permissions are separate requirements.
- The LAPS password stored in AD must be verified by a real local login, not just by seeing a value in ADUC.
