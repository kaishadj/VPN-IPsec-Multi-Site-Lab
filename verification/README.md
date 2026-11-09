# Captured verification

These files preserve outputs shared during the guided lab. They are snapshots, not live checks of the `.pkt` file.

| Evidence | Result |
| --- | --- |
| [`c-to-b-ping.txt`](c-to-b-ping.txt) | PC C reached PC B, 4/4 replies |
| [`c-isakmp-sa.txt`](c-isakmp-sa.txt) | C had active IKE associations with A and B |
| [`ipsec-observations.md`](ipsec-observations.md) | C ↔ A packet counters and matching C ↔ B ESP SPI |

The full C → A and A → B ping transcripts were not retained. Those tests were reported successful during the session. Re-run them from the saved lab for fresh evidence.
