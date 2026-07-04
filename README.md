# Branch Office VLAN Segmentation Lab

## Overview
This project simulates a small branch office network using Cisco Packet Tracer. The lab demonstrates VLAN segmentation, 802.1Q trunking, inter-VLAN routing using router-on-a-stick, and basic security enforcement with an extended ACL.

## Objectives
- Create separate VLANs for users, servers, and guest devices
- Configure trunk links between switches and the router
- Enable inter-VLAN routing with router subinterfaces
- Apply an ACL to block guest access to the server network
- Verify allowed and denied traffic with connectivity tests

## Topology
- 1 Router: R1
- 2 Switches: SW1, SW2
- 4 User PCs
- 1 Server
- 1 Guest PC

## VLAN Plan
| VLAN | Name | Subnet | Gateway | Purpose |
|---|---|---|---|---|
| 10 | USERS | 192.168.10.0/24 | 192.168.10.1 | Employee endpoints |
| 20 | SERVERS | 192.168.20.0/24 | 192.168.20.1 | Internal server segment |
| 30 | GUEST | 192.168.30.0/24 | 192.168.30.1 | Guest access |
| 99 | MGMT | 192.168.99.0/24 | 192.168.99.1 | Management VLAN |

## Port Assignments
### SW1
- Fa0/1-PC1-VLAN10
- Fa0/2-PC2-VLAN10
- Fa0/10-Server1-VLAN20
- Fa0/23-Trunk to SW2
- Fa0/24-Trunk to R1

### SW2
- Fa0/1-PC3-VLAN10
- Fa0/2-PC4-VLAN10
- Fa0/10-Guest-PC-VLAN30
- Fa0/24-Trunk to SW1

## ACL Policy
The Guest VLAN is not allowed to access the Server VLAN.

```cisco
ip access-list extended GUEST_BLOCK_SERVERS
 deny ip 192.168.30.0 0.0.0.255 192.168.20.0 0.0.0.255
 permit ip any any
```

Applied inbound on:
```cisco
interface g0/0.30
 ip access-group GUEST_BLOCK_SERVERS in
```

## Key Skills Demonstrated
- VLAN creation and port assignment
- 802.1Q trunk configuration
- Inter-VLAN routing
- Extended ACL configuration
- Network verification and troubleshooting

## Verification
### Successful tests
- Users VLAN to Servers VLAN
- Users VLAN devices across both switches
- Guest VLAN to default gateway

### Blocked tests
- Guest VLAN to Servers VLAN

## Files in this Repo
- `topology.png` - network diagram
- `configs/R1.txt` - router configuration
- `configs/SW1.txt` - switch 1 configuration
- `configs/SW2.txt` - switch 2 configuration
- `screenshots/` - verification images and command outputs

## Lessons Learned
This lab reinforced the importance of proper VLAN assignment, correct trunk configuration, and applying ACLs on the correct interface and direction. It also showed how segmentation improves security while still allowing required connectivity between trusted networks.

## Future Improvements
- Add DHCP for each VLAN
- Add NAT/PAT for simulated internet access
- Add a management SVI for switch administration
- Add SSH for secure remote management
