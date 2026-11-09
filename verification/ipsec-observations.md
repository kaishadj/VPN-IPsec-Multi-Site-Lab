# IPsec observations from the guided lab

## C ↔ A

On OC-SITE-C, `show crypto ipsec sa` identified local LAN `192.168.3.0/24`, remote LAN `192.168.1.0/24`, and peer `62.59.1.1`. The captured counters were `#pkts encrypt: 5` and `#pkts decrypt: 4`. An inbound and outbound ESP SA were shown as `ACTIVE`.

The same captured output showed `#send errors 1`. Its `#recv errors` value later rose from 6 to 10 while testing C ↔ B. The reason for those receive errors was not established. The pings reported in the lab succeeded, so the counters are preserved here as an observation rather than presented as a clean zero-error result.

## C ↔ B

OC-SITE-C showed an outbound ESP SPI of `0x6689BAC0`. OC-SITE-B showed an inbound ESP SA with the **same SPI**, `0x6689BAC0`, for local endpoint `59.62.1.1` and remote endpoint `80.59.62.1`. Both routers' crypto maps pointed to each other, used `OCSET`, and had reverse VPN selectors (`C-TO-B` and `B-TO-C`). PC C's ping to PC B succeeded 4/4.

The pasted `show crypto ipsec sa` excerpt did not include separate C ↔ B encrypt/decrypt counters. Re-run the ping and inspect the complete SA output for a fresh counter snapshot if needed.
