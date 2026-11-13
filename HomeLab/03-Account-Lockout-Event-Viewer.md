# 03 — Account Lockout + Event Viewer

## Goal

Configure an account lockout policy, deliberately generate failed logons, verify the account locks, then inspect the security events.

## Additional workstation

A second Windows workstation was created as `WS-02`.

Initial LAB address:
- IP: `10.10.10.30`
- subnet: `255.255.255.0`
- DNS: `10.10.10.10`

It was joined to `ad.malunas.test`.

## Policy

Account lockout settings were configured through Group Policy.

After policy configuration, `gpupdate /force` was used so the workstation received the updated policy.

## Event Viewer

The important learning point was that domain authentication evidence belongs on the system performing the authentication / domain role, not necessarily on the workstation where the bad password was typed.

Verified Security events included:
- Event ID `4625` — failed logon
- Event ID `4740` — account locked out

## Lessons

- Event Viewer is historical evidence, not a live "problem detector".
- Start with timestamp -> provider/source -> event ID -> level -> message.
- The same user action may produce relevant evidence on a different machine than the client where the action originated.
