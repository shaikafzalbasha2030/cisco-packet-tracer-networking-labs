# Small-Office VLAN, DHCP, DNS & Inter-VLAN Routing Lab

Cisco Packet Tracer lab implementing a segmented small-office network using router-on-a-stick inter-VLAN routing, centralized router DHCP pools, internal DNS (`intranet.office.local`), and HTTP service verification.

## Topology & Key Links
- **Router (`Office-R1` / 1941):** `GigabitEthernet0/0` trunk link with sub-interfaces `.10`, `.20`, and `.30`
- **Switch (`Switch0` / 2960):** `FastEthernet0/6` configured as 802.1Q trunk to `Office-R1`; access ports assigned to department VLANs
- **Server (`Server0`):** Hosts DNS A record for `intranet.office.local` (`192.168.30.10`) and HTTP service
- **Endpoints (`PC0`–`PC3`):** Clients receiving dynamic IPv4, subnet mask, default gateway, and DNS settings via DHCP

## Addressing & VLAN Plan
| VLAN | Role | Subnet | Default Gateway | DHCP / Service Notes |
| :--- | :--- | :--- | :--- | :--- |
| 10 | HR / Admin | `192.168.10.0/24` | `192.168.10.1` | `HR_pool` (`192.168.10.1`–`.9` excluded) |
| 20 | IT / Eng | `192.168.20.0/24` | `192.168.20.1` | `IT_Pool` (`192.168.20.1`–`.9` excluded) |
| 30 | Servers | `192.168.30.0/24` | `192.168.30.1` | `Server0` at `192.168.30.10` (`intranet.office.local`) |

## Verification & Troubleshooting Highlights
- **Trunk Port Discovery:** Used `show cdp neighbors` on `Office-R1` to confirm `Gig0/0` connected to `Switch0` `Fa0/6`, then configured `switchport mode trunk` on `Fa0/6`.
- **DHCP & DNS:** Verified dynamic leases across `PC0`–`PC3`, resolved `intranet.office.local` to `192.168.30.10`, and confirmed HTTP access in `PC0`'s browser.
- **Inter-VLAN Routing:** Verified 0% packet loss pings between `192.168.10.x` and `192.168.20.x` hosts across `Office-R1`.
