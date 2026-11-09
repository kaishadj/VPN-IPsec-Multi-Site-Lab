# Design notes

## Physical and logical layout

The three site routers connect to one WAN router. Each site router also connects to one PC on its own `/24` LAN. Site C uses a single-mode fiber link with HWIC-1GE-SFP and GLC-LH-SMD modules because the exercise specifies an optical connection to WAN. The A and B WAN links use copper.

The WAN router does not terminate IPsec. Site A, Site B and Site C each use their WAN-facing address as an IPsec endpoint. There are three protected site pairs: A ↔ B, A ↔ C and B ↔ C.

## Addressing and routing

The site LANs are `192.168.1.0/24`, `192.168.2.0/24` and `192.168.3.0/24`. Each PC's default gateway is the `.254` address on its site router. Each site router has a default route to the adjacent WAN interface:

| Site | Next hop |
| --- | --- |
| A | `62.59.1.2` |
| B | `59.62.1.2` |
| C | `80.59.62.2` |

## NAT and VPN selection

The named `NAT` ACL is used by `ip nat inside source list NAT ... overload`. In that context, `deny` means *do not translate this flow*. Each site denies NAT selection for its other two site LANs, then permits its remaining outbound traffic for translation.

VPN ACLs have a different job. For example, `C-TO-B` permits traffic sourced from `192.168.3.0/24` and destined for `192.168.2.0/24`. `match address C-TO-B` on C's crypto map uses that ACL to select packets for the C ↔ B tunnel. B has the reverse selector, `B-TO-C`.

The two decisions work together: the private source and destination remain unchanged for VPN matching, and IPsec then encapsulates the protected packet between the routers' WAN-facing addresses.

## IKE and IPsec

The three site routers share an IKEv1 policy: pre-shared-key authentication, AES-256 encryption, SHA hashing, Diffie-Hellman group 5 and a 21,600-second phase 1 lifetime. Each site pair has a matching lab-only pre-shared key. Phase 2 uses `OCSET` with ESP AES-256 and ESP SHA-HMAC; each crypto map entry has a 3,600-second security-association lifetime.

The sequence number identifies a tunnel within a crypto map. A and B use sequence 10 for their original tunnel. A and C use sequence 20, and B and C use sequence 30. Site C applies the shared `OCMAP` to its fiber interface G0/1/0; A and B apply `OCMAP` to G0/1.
