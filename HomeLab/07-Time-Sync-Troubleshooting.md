# 07 — Windows Time Troubleshooting

## Trigger

The remote health check exposed Windows Time warnings:
- Event ID `129`
- Event ID `24`

WS2 reported problems obtaining valid domain time.

## Investigation

### Time zone

WS2 used `GMT Standard Time`.

DC was found using `Pacific Standard Time` and was changed to GMT.

Important correction:
time zone and time synchronization are different concepts. The time-zone mismatch was not proven to be the root cause of the domain sync warnings.

### Active time source

`w32tm /query /source`

WS2 initially reported:

`VM IC Time Synchronization Provider`

This showed that Hyper-V Integration Services was acting as the active source instead of the expected domain hierarchy.

Hyper-V Time Synchronization integration was disabled for the lab VMs to make the Windows domain time hierarchy explicit.

After the change:
- WS2 source -> `MM-DC01.ad.malunas.test`
- DC source -> `Local CMOS Clock`

`w32tm /query /configuration` showed:
- `Type = NT5DS`

This confirmed the domain member was configured for the domain hierarchy.

## Result

No new Event ID 24 / 129 warnings appeared during the observed period after the change.

## Root-cause confidence

Strong finding:
Hyper-V time synchronization was masking the expected WS2 domain time source.

Not proven:
that Hyper-V alone caused every earlier warning.

## Production note

`Local CMOS Clock` is acceptable for this small isolated lab checkpoint, but a production domain hierarchy would normally have the PDC Emulator use a reliable upstream time source.
