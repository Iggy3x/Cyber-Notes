# 05 — WinRM / PowerShell Remoting

## Goal

Administer `WS-02` remotely from `MM-WS01` without opening the WS2 desktop.

Model:

`WS1 admin -> WinRM / PowerShell Remoting -> WS2`

## GPO

WinRM policy was configured on the `Workstations` scope.

The IPv4 filter initially caused trouble. A single address had to be entered using the range syntax used by the policy:

`10.10.10.30-10.10.10.30`

The WinRM listener initially showed no usable `ListeningOn` address. After correcting the filter, it listened on the WS2 LAB address.

## Firewall + service

Remote management also required:
- Windows Remote Management inbound firewall access
- WinRM service running
- WinRM service startup configured through GPO

## Verification

From WS1:
- `Test-WSMan WS-02`
- `Enter-PSSession -ComputerName WS-02`

Inside the remote session:
- `hostname` -> `WS-02`
- `whoami` -> `AD\Bones`
- network/service checks worked

One-time remote execution was also verified:

`Invoke-Command -ComputerName WS-02 -ScriptBlock { Get-Service }`

Important:
`Invoke-Command` was run from the WS1 shell, not from inside the existing WS2 interactive session.

## Lessons

- WinRM policy, service, listener and firewall are separate layers.
- A GPO can exist without actually being applied.
- Verify effective policy with `gpresult`.
- A listener with no usable listening address is different from a service merely being "Running".
