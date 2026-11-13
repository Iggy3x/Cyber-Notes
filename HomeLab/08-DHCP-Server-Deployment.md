# 08 — DHCP Server Deployment

## Goal

Deploy DHCP on `MM-DC01` while keeping infrastructure addresses static.

## Final configuration

DC:
- `10.10.10.10/24` static
- DNS: `10.10.10.10`
- no gateway on `LAB-Internal`

DHCP:
- service: Running
- scope: `10.10.10.100-10.10.10.199`
- subnet: `255.255.255.0`
- option 006 DNS Server: `10.10.10.10`
- option 015 DNS Domain Name: `ad.malunas.test`
- no Router/default-gateway option
- authorized in Active Directory

## Major mistakes and fixes

### DHCP Server vs DHCP Client

`DHCP Enabled: No` in `ipconfig /all` was initially misread as "DHCP Server is disabled".

Actual meaning:
the NIC itself is not using DHCP as a client.

The DC was accidentally changed to automatic addressing and had to be restored to static configuration.

### Bad subnet mask

At one point the DC used:

`255.255.255.255` (`/32`)

Correct lab mask:

`255.255.255.0` (`/24`)

### Bad first scope

Initial scope:
`10.10.10.10-10.10.10.22`

This overlapped static infrastructure addresses.

Final pool:
`10.10.10.100-10.10.10.199`

### DHCP authorization

`Get-DhcpServerInDC` initially showed the DHCP server authorized with an incorrect IP:

`255.0.0.1`

The authorization entry was corrected and verified as:

`mm-dc01.ad.malunas.test -> 10.10.10.10`

## Lessons

- DHCP Server != DHCP Client.
- A DC should keep a stable static address.
- Scope design should avoid static infrastructure addresses.
- Scope Active != DHCP server authorized in AD.
- The isolated LAB network does not need a Router option because there is no router/default gateway.
