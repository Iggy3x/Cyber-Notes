# 06 — WS2 Remote Health Check

## Goal

From WS1 only, collect a useful health snapshot of `WS-02`.

## Verified

Identity:
- hostname: `WS-02`
- remote user: `AD\Bones`

Network at the time:
- IP: `10.10.10.30`
- DNS server: `10.10.10.10`
- LAB adapter: Up

System:
- uptime obtained through `Get-ComputerInfo` / CIM
- PowerShell version was 5.1, so `Get-Uptime` was unavailable
- disk C: approximately `59.02 GB` total, `41.07 GB` free
- WinRM: Running

Processes:
- `wsmprovhost` was the top RAM consumer during the remote session, which is consistent with WinRM / PowerShell Remoting activity

Events:
- System events were retrieved remotely
- Windows Time warnings became the next real troubleshooting task

## Corrections learned

- A/AAAA DNS record != DNS server address.
- `OSMaxNumberOfProcesses` is not RAM.
- `chkdsk` can show disk information, but `Get-Volume` is better for a health-check free-space check.
- Sorting processes requires sorting by memory usage, not by process name.

## Key commands

`Get-NetIPConfiguration`  
`Get-NetAdapter`  
`Get-ComputerInfo`  
`Get-CimInstance -ClassName Win32_OperatingSystem`  
`Get-Volume`  
`Get-Service`  
`Get-Process`  
`Get-WinEvent`
