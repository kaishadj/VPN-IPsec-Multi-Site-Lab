# Open and verify the lab

1. Open [`VPN-IPsec-Multi-Site-Lab.pkt`](../VPN-IPsec-Multi-Site-Lab.pkt) in Cisco Packet Tracer.
2. Check that the three PCs have the IP addresses and `.254` gateways in the [main IP plan](../README.md#topology-and-ip-plan).
3. From PC C's **Desktop → Command Prompt**, run `ping 192.168.1.1` and `ping 192.168.2.1`. From PC A, run `ping 192.168.2.1`. Repeat a first ping if IKE negotiation causes an initial timeout.
4. On OC-SITE-C, run `show crypto isakmp sa`. Expect peers `62.59.1.1` and `59.62.1.1` in `QM_IDLE ACTIVE` after traffic has crossed both tunnels.
5. Run `show crypto ipsec sa` on OC-SITE-C and OC-SITE-B. Check the protected LAN selectors, peer addresses, ESP SAs and encrypted/decrypted packet counters. Packet Tracer may present several SAs under one long output; read it through to the router prompt.
6. Run `show crypto map` on each site router to inspect peer addresses, transform sets, VPN ACLs and applied interfaces. Use `show access-lists NAT` to confirm the VPN exclusions appear before the general NAT permit.

The [verification files](../verification/) contain outputs captured during the guided lab. Packet counters are snapshots and will change with traffic and after a reload. The course's Packet Tracer example notes that digest/verify counters may stay at zero in the simulator.
