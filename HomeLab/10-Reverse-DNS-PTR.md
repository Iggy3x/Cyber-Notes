# 10 — Reverse DNS / PTR

## Why this task existed

DHCP/DNS-related events showed PTR registration failures.

Forward DNS already worked:

`WS-02.ad.malunas.test -> 10.10.10.101`

Reverse DNS did not have the required zone.

## Core concept

A record:

`hostname -> IP`

PTR record:

`IP -> hostname`

Forward lookup asks:

`I know the name; what is the IP?`

Reverse lookup asks:

`I know the IP; what host is this?`

## Network ID

LAB network:

`10.10.10.0/24`

- `10.10.10.10` = host
- `10.10.10.101` = host
- `10.10.10.0` = network ID

## Reverse Lookup Zone

Created on MM-DC01:

`10.10.10.in-addr.arpa`

Type:
AD-integrated Primary Zone

PTR records were not manually created during the task.

After the zone existed, PTR records were visible, including:

`10.10.10.101 -> WS-02.ad.malunas.test`

## Important accuracy note

The PTR appeared without manual creation after the reverse zone existed.

The task did not conclusively isolate whether the successful registration was performed by:
- the WS2 DNS client, or
- DHCP Server acting on behalf of the client.

Therefore the clean note should say:

`PTR registration succeeded automatically after the reverse zone existed.`

It should not claim a specific actor unless separately proven.

## Event evidence

Client-side:
- DNS Client Events
- Event ID `8010`
- PTR registration failure while WS2 still used `10.10.10.30`

Server-side:
- DHCP Server event associated with `10.10.10.101 / WS-02`
- PTR registration failure before reverse DNS was available

After the reverse zone was created, no new matching PTR failure was observed during the final check.

## Verification

From WS1:

`nslookup 10.10.10.101`

Resolved to:

`WS-02.ad.malunas.test`

Final state:
- forward lookup works
- reverse lookup works
- PTR exists
- reverse zone exists

Status: COMPLETE
