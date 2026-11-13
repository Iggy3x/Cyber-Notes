# Windows / Active Directory Home Lab — Command Cheat Sheet

Only commands actually used in the lab.

## Core

`hostname` — computer name  
`whoami` — current user  
`whoami /groups` — effective logon-token groups  
`ipconfig /all` — IP, mask, DNS, DHCP and lease information  
`Get-NetIPConfiguration` — PowerShell network overview  
`Get-NetAdapter` — adapter status / MAC / speed  
`ping <host>` — ICMP test  
`nslookup <name-or-IP>` — forward or reverse DNS lookup  
`Test-NetConnection <host> -Port <port>` — TCP connectivity  
`gpupdate /force` — refresh Group Policy  
`gpresult /scope computer /r` — effective computer GPOs  

## Services / system

`Get-Service`  
`Get-Service WinRM`  
`Get-ComputerInfo`  
`Get-CimInstance -ClassName Win32_OperatingSystem`  
`Get-Volume`  
`Get-Process`  
`Get-WinEvent System`  
`$PSVersionTable`

## RSAT / AD

`dsa.msc` — Active Directory Users and Computers  
`dnsmgmt.msc` — DNS Manager  
`Get-WindowsCapability -Name RSAT* -Online`

## LAPS

`Invoke-LapsPolicyProcessing`  
`Update-LapsADSchema`  
`Set-LapsADComputerSelfPermission -Identity "OU=Workstations,DC=ad,DC=malunas,DC=test"`  
`Reset-LapsPassword`

## WinRM / remoting

`Test-WSMan WS-02`  
`Enter-PSSession -ComputerName WS-02`  
`Exit-PSSession`  
`Invoke-Command -ComputerName WS-02 -ScriptBlock { Get-Service }`  
`winrm enumerate winrm/config/listener`

WinRM HTTP port used in this lab: `5985`

## Windows Time

`Get-TimeZone`  
`Set-TimeZone -Id "GMT Standard Time"`  
`w32tm /query /source`  
`w32tm /query /configuration`

## DHCP

`Get-Service DHCPServer` — run on the DHCP server  
`Get-DhcpServerInDC` — authorized DHCP servers in AD  
`ipconfig /all` — client-side lease / DHCP data

Current lab DHCP:
- server: `10.10.10.10`
- scope: `10.10.10.100-10.10.10.199`

## DNS / PTR

Forward:
`nslookup WS-02`

Reverse:
`nslookup 10.10.10.101`

## Notes

- `DHCPServer` service belongs to the server role; workstation DHCP client is a different component.
- Do not use old static WS2 address `10.10.10.30` as the default target in generic connectivity examples. WS2 now uses DHCP.
- Prefer hostname `WS-02` when the task is not specifically about IP-level troubleshooting.
