# 09 — WS2 Static-to-DHCP Migration

## Goal

Move WS2 from static `10.10.10.30` to DHCP remotely from WS1 and verify the endpoint still works.

## Baseline

WS2:
- hostname: `WS-02`
- IP: `10.10.10.30`
- mask: `255.255.255.0`
- DNS: `10.10.10.10`
- adapter: Up
- MAC: `00-15-5D-38-01-06`

The MAC was saved before the disruptive change.

## Change

While remotely connected to WS2:

`Set-NetIPInterface -InterfaceAlias "Ethernet" -Dhcp Enabled`

The remote session dropped immediately because the IP changed.

## Finding WS2 again

Using ARP from WS1 and the saved MAC:

`00-15-5D-38-01-06 -> 10.10.10.101`

This proved WS2 had received an address inside the DHCP pool.

DNS Manager also showed:

`WS-02 -> 10.10.10.101`

## Important accuracy note: DNS

Final WS2 DNS server was verified as:

`10.10.10.10`

However, the recorded change explicitly enabled DHCP for the IP interface. The notes do not conclusively prove that the DNS client setting itself was switched from static DNS to DHCP-provided DNS.

Therefore the safe statement is:

`WS2 had the correct DNS server after migration.`

Not:

`DNS was proven to have been obtained from DHCP option 006.`

## WinRM incident

After the address change:
- DNS resolved WS2 to `.101`
- TCP 5985 initially failed
- PowerShell Remoting failed

A stale WinRM GPO IPv4 filter still referenced:

`10.10.10.30-10.10.10.30`

It was updated for the new address.

WinRM did not recover immediately. Later, `gpupdate /force` was run locally on WS2, after which:
- `Test-WSMan WS-02` succeeded
- TCP 5985 succeeded
- `Enter-PSSession -ComputerName WS-02` succeeded

### Root-cause confidence

Strongly supported:
the stale WinRM IPv4 filter plus delayed policy application caused the remoting failure.

Not fully reproduced:
the failure was not recreated in a controlled before/after experiment, so the note should not present this as mathematically proven.

### Technical debt

The WinRM filter was again tied to a single client IP. Because WS2 now uses DHCP, another future address change could create the same problem. This is a lab design issue to revisit later, not something that was fixed during this task.

## DHCP lease verification

DHCP Manager showed:
- client IP: `10.10.10.101`
- name: `ws-02.ad.malunas.test`
- Unique ID present
- lease expiration: `10/29/2026 7:19:05`

`ipconfig /all` also exposed client-side lease timing.

## Event Viewer

No direct "lease granted to WS2" event was conclusively identified in Event Viewer.

What was found:
- server-side DHCP/DNS event tied `10.10.10.101` to `WS-02`
- PTR registration failed because reverse DNS was not yet configured

Important:
that event is evidence related to the DHCP/DNS transaction, but it is not the same thing as a direct lease-grant audit record.

## Final health check

From WS1:
- hostname: `WS-02`
- user: `AD\Bones`
- IP: `10.10.10.101`
- DNS: `10.10.10.10`
- adapter: Up
- WinRM: Running
- remote session: working
- recent System events: no new serious network/WinRM failure observed

Status: COMPLETE
