# Enterprise Network Infrastructure Using Cisco Packet Tracer

## Project Overview

This project implements a complete enterprise multi-site network infrastructure using Cisco Packet Tracer.

The network is designed with a Headquarters (HQ), Branch Office, routed WAN infrastructure, wireless networks, IoT devices, security controls, dynamic routing, DHCP services, NAT/PAT and a self-contained Internet simulation.

The project demonstrates practical enterprise networking concepts across Layer 2, Layer 3, network security, wireless networking, IoT networking and Internet-edge connectivity.

The final implementation was validated through device configuration, routing tables, DHCP bindings, wireless client addressing, gateway tests, NAT translations and end-to-end connectivity to the simulated Internet endpoint `8.8.8.8`.

---

## Project Objectives

The main objectives of this project are:

- Design an enterprise multi-site network.
- Implement VLAN-based network segmentation.
- Configure inter-VLAN routing.
- Implement router-on-a-stick at the branch.
- Implement OSPF dynamic routing.
- Configure DHCP services for branch VLANs.
- Implement Guest network isolation using ACLs.
- Secure network device management using SSH Version 2.
- Implement EtherChannel using LACP.
- Configure Port Security.
- Configure PortFast and BPDU Guard.
- Implement DHCP Snooping where stable in the Packet Tracer environment.
- Implement Dynamic ARP Inspection.
- Deploy HQ wireless networking.
- Deploy Branch wireless networking.
- Deploy a dedicated IoT wireless network.
- Implement NAT/PAT.
- Build a self-contained simulated Internet environment.
- Validate the complete network from endpoint to Internet simulation.
- Document troubleshooting and recovery procedures.

---

# Network Architecture

## Headquarters

The Headquarters network is centered around `SW-CORE`.

Main HQ components:

- SW-CORE
- R-HQ
- AP-HQ
- AP-IOT
- HQ wired endpoints
- HQ Wi-Fi clients
- IoT devices
- Server/DMZ environment

SW-CORE acts as the HQ Layer-3 aggregation and routing boundary.

The HQ traffic path is:

HQ Endpoint → SW-CORE → R-HQ → R-ISP → R-INTERNET → 8.8.8.8

HQ Wi-Fi traffic uses VLAN 90.

HQ IoT traffic uses VLAN 80.

---

## Branch Office

The Branch network consists of:

- R-BR
- SW-BR1
- SW-BR2
- AP-BR
- Branch wired clients
- Branch wireless clients

The Branch traffic path is:

Branch Endpoint → SW-BR1/SW-BR2 → R-BR → R-ISP → R-INTERNET → 8.8.8.8

SW-BR1 and SW-BR2 are connected using LACP EtherChannel.

R-BR provides router-on-a-stick inter-VLAN routing and DHCP services.

---

## Internet Simulation

The Internet simulation consists of:

- R-ISP
- R-INTERNET
- R-INTERNET Loopback0

The simulated Internet endpoint is:

`8.8.8.8/32`

R-ISP performs NAT/PAT for private enterprise addresses.

The Internet endpoint is used to validate routing, NAT/PAT and return traffic.

---

# Device Inventory

| Device | Role |
|--------|------|
| SW-CORE | HQ Layer-3 core / aggregation switch |
| SW-BR1 | Branch access switch |
| SW-BR2 | Branch access switch |
| R-HQ | HQ routed edge |
| R-BR | Branch router and DHCP server |
| R-ISP | NAT/PAT Internet edge |
| R-INTERNET | Simulated Internet router |
| AP-HQ | HQ wireless access point |
| AP-BR | Branch wireless access point |
| AP-IOT | HQ IoT access point |

---

# VLAN Design

VLAN segmentation is used to create logical business, security and operational boundaries.

## HQ VLANs

| VLAN | Name | Network | Gateway |
|------|------|---------|---------|
| 10 | ADMIN / USERS | 10.10.10.0/24 | 10.10.10.1 |
| 20 | HR | 10.10.20.0/24 | 10.10.20.1 |
| 30 | FINANCE | 10.10.30.0/24 | 10.10.30.1 |
| 40 | IT | 10.10.40.0/24 | 10.10.40.1 |
| 50 | SALES | 10.10.50.0/24 | 10.10.50.1 |
| 60 | GUEST | 10.10.60.0/24 | 10.10.60.1 |
| 70 | SERVER-DMZ | 10.10.70.0/24 | 10.10.70.1 |
| 80 | IoT | 10.10.80.0/24 | 10.10.80.1 |
| 90 | WIFI | 10.10.90.0/24 | 10.10.90.1 |
| 99 | MANAGEMENT | 10.10.99.0/24 | 10.10.99.1 |

## Branch VLANs

| VLAN | Name | Network | Gateway |
|------|------|---------|---------|
| 10 | USERS | 10.20.10.0/24 | 10.20.10.1 |
| 40 | IT | 10.20.40.0/24 | 10.20.40.1 |
| 60 | GUEST | 10.20.60.0/24 | 10.20.60.1 |
| 80 | IoT | 10.20.80.0/24 | 10.20.80.1 |
| 90 | WIFI | 10.20.90.0/24 | 10.20.90.1 |
| 99 | MANAGEMENT | 10.20.99.0/24 | 10.20.99.1 |

---

# Routed Point-to-Point Links

The routed backbone uses /30 networks.

| Connection | Local Address | Peer Address |
|------------|---------------|--------------|
| R-BR G0/1 ↔ R-ISP G0/1 | 10.10.252.1/30 | 10.10.252.2/30 |
| R-HQ G0/1 ↔ R-ISP G0/0 | 10.10.253.1/30 | 10.10.253.2/30 |
| SW-CORE ↔ R-HQ G0/0 | 10.10.254.2/30 | 10.10.254.1/30 |
| R-ISP G0/2 ↔ R-INTERNET G0/0 | 10.10.255.1/30 | 10.10.255.2/30 |

R-INTERNET Loopback0:

`8.8.8.8/32`

---

# Technologies Used

- Cisco Packet Tracer
- VLAN
- Inter-VLAN Routing
- Layer-3 Switching
- Router-on-a-Stick
- OSPF
- DHCP
- Extended ACL
- SSH Version 2
- EtherChannel
- LACP
- PVST
- Port Security
- PortFast
- BPDU Guard
- DHCP Snooping
- Dynamic ARP Inspection
- Wireless Networking
- IoT Networking
- NAT
- PAT
- Internet Simulation

---

# SW-CORE Configuration

SW-CORE is the central Layer-3 device for HQ.

Layer-3 routing is enabled using:

`ip routing`

The switch provides gateway addresses for the HQ VLANs using Switch Virtual Interfaces.

Example SVI structure:

- VLAN 10 → 10.10.10.1
- VLAN 20 → 10.10.20.1
- VLAN 30 → 10.10.30.1
- VLAN 40 → 10.10.40.1
- VLAN 50 → 10.10.50.1
- VLAN 60 → 10.10.60.1
- VLAN 70 → 10.10.70.1
- VLAN 80 → 10.10.80.1
- VLAN 90 → 10.10.90.1
- VLAN 99 → 10.10.99.1

Verification commands used include:

`show ip interface brief`

`show ip route`

---

# SW-CORE OSPF

OSPF Process 10 with Area 0 is used.

SW-CORE uses Router ID:

`4.4.4.4`

The OSPF network includes the routed core link and HQ VLAN networks.

OSPF provides learned routes for remote enterprise networks.

The project uses OSPF for internal enterprise routing while static default routes are used for Internet-bound traffic.

---

# SW-CORE Default Route

The HQ core uses:

`ip route 0.0.0.0 0.0.0.0 10.10.254.1`

This sends unknown destinations toward R-HQ.

Verification:

`show ip route 0.0.0.0`

Expected:

`S* 0.0.0.0/0 [1/0] via 10.10.254.1`

---

# Branch Switching

SW-BR1 and SW-BR2 form the branch Layer-2 access layer.

## SW-BR1 Access Ports

| Port | VLAN | Role |
|------|------|------|
| Fa0/1 | 10 | Branch user |
| Fa0/2 | 10 | Branch user |
| Fa0/3 | 40 | Branch IT |

## SW-BR2 Access Ports

| Port | VLAN | Role |
|------|------|------|
| Fa0/1 | 10 | Branch user |
| Fa0/2 | 10 | Branch user |
| Fa0/3 | 60 | Branch Guest |
| Fa0/4 | 90 | Branch Wi-Fi client context |
| Fa0/10 | 90 | AP-BR uplink |

## HQ IoT Uplink

SW-CORE Fa0/9 is used for AP-IOT.

Fa0/9 belongs to:

`VLAN 80`

---

# EtherChannel and LACP

SW-BR1 and SW-BR2 are connected using two physical links.

The links are bundled into:

`Port-channel4`

LACP is configured using active mode.

Member interfaces:

- Fa0/23
- Fa0/24

The trunk carries:

`10,40,60,80,90,99`

Representative configuration:

`channel-group 4 mode active`

Verification:

`show etherchannel summary`

`show interfaces trunk`

The final validated state showed Group 4 with LACP and the member links bundled into the port-channel.

---

# Spanning Tree

The switches use PVST.

Host-facing access ports use:

`spanning-tree portfast`

and:

`spanning-tree bpduguard enable`

PortFast reduces host-facing convergence delay.

BPDU Guard protects edge ports from unexpected BPDUs.

Edge protections are applied to host-facing ports rather than trunks or routed links.

---

# R-BR Router-on-a-Stick

R-BR provides branch inter-VLAN routing.

Subinterfaces are configured for:

- VLAN 10 → 10.20.10.1
- VLAN 40 → 10.20.40.1
- VLAN 60 → 10.20.60.1
- VLAN 80 → 10.20.80.1
- VLAN 90 → 10.20.90.1
- VLAN 99 → 10.20.99.1

Each subinterface uses 802.1Q encapsulation.

The WAN interface:

`G0/1 = 10.10.252.1/30`

---

# R-BR OSPF

R-BR participates in OSPF Process 10.

The WAN segment:

`10.10.252.0/30`

is advertised in Area 0.

The verified OSPF neighbor was:

Neighbor ID: `2.2.2.2`

State: `FULL/BDR`

Address: `10.10.252.2`

Interface: `GigabitEthernet0/1`

Verification:

`show ip ospf neighbor`

`show ip route`

---

# R-BR Default Route

Branch Internet traffic uses:

`ip route 0.0.0.0 0.0.0.0 10.10.252.2`

R-ISP is the next hop for branch Internet traffic.

---

# DHCP Services

R-BR provides DHCP services for the branch networks.

Excluded address ranges reserve infrastructure addresses.

Examples:

`10.20.10.1 - 10.20.10.20`

`10.20.40.1 - 10.20.40.20`

`10.20.60.1 - 10.20.60.20`

`10.20.80.1 - 10.20.80.20`

`10.20.90.1 - 10.20.90.20`

DHCP pools include:

- BRANCH-USERS
- BRANCH-IT
- BRANCH-GUEST
- BRANCH-IOT
- BRANCH-WIFI

Configured DNS value:

`8.8.8.8`

Verification:

`show ip dhcp binding`

Verified leases included addresses such as:

- 10.20.10.21
- 10.20.10.22
- 10.20.10.24
- 10.20.40.21
- 10.20.60.22
- 10.20.90.22
- 10.20.90.23

---

# Guest Network Isolation

HQ Guest VLAN:

`VLAN 60`

Network:

`10.10.60.0/24`

Gateway:

`10.10.60.1`

An extended ACL named:

`ACL-GUEST-IN`

is applied inbound on the Guest SVI.

The policy blocks Guest traffic from directly reaching internal enterprise networks such as:

- 10.10.10.0/24
- 10.10.20.0/24
- 10.10.30.0/24
- 10.10.40.0/24
- 10.10.50.0/24
- 10.10.70.0/24
- 10.10.80.0/24
- 10.10.90.0/24
- 10.10.99.0/24

The ACL then permits other destinations.

Validation from the Guest network showed that internal gateway addresses such as:

`10.10.10.1`

`10.10.20.1`

`10.10.70.1`

were blocked as intended.

---

# SSH Management Security

Network device administration uses SSH Version 2.

Configured controls include:

- SSH Version 2
- Local authentication
- Privilege 15 admin account
- Domain `corp.local`
- SSH-only VTY transport
- Login banner

Verification:

`show ip ssh`

Expected:

`SSH Enabled - version 2.0`

Actual passwords are not stored in this repository.

---

# Port Security

Host-facing branch access ports use Port Security with sticky MAC learning.

Example configuration uses:

- switchport mode access
- switchport port-security
- switchport port-security mac-address sticky
- switchport port-security violation restrict
- spanning-tree portfast

Protected branch ports include:

- SW-BR1 Fa0/1
- SW-BR1 Fa0/2
- SW-BR1 Fa0/3
- SW-BR2 Fa0/1
- SW-BR2 Fa0/2
- SW-BR2 Fa0/3
- SW-BR2 Fa0/4

Verification:

`show port-security`

One stale sticky MAC entry on SW-BR2 Fa0/1 was removed during troubleshooting so that the expected endpoint could use the correct access-port association.

---

# DHCP Snooping

DHCP Snooping was enabled on branch VLANs:

`10,40,60,80,99`

Selected access ports used a rate limit of:

`10 packets per second`

Trusted uplinks were configured where supported.

## Packet Tracer Compatibility Exception

During branch Wi-Fi testing, SW-BR2 Fa0/4 repeatedly entered an err-disabled state because of DHCP Snooping rate-limit behavior.

Observed messages included:

`DHCP_SNOOPING_ERRDISABLE_WARNING`

and:

`ERR_DISABLE: dhcp-rate-limit error detected on Fa0/4`

The final stable state disabled DHCP Snooping globally on SW-BR2 while configuration lines for the other VLANs remained visible.

SW-BR1 retained DHCP Snooping for VLANs 10, 40, 60, 80 and 99.

This behavior is documented as a Packet Tracer compatibility workaround and should not be presented as the preferred production design without testing on the target platform.

Verification:

`show ip dhcp snooping`

---

# Dynamic ARP Inspection

Dynamic ARP Inspection was configured for:

- VLAN 10
- VLAN 40
- VLAN 60
- VLAN 80
- VLAN 90
- VLAN 99

Configuration:

`ip arp inspection vlan 10,40,60,80,90,99`

Verification:

`show ip arp inspection`

The final validation showed the configured VLANs in Active state.

DAI complements DHCP Snooping by using trusted binding information for ARP validation.

---

# HQ Wi-Fi

AP-HQ provides HQ user wireless access.

Wireless configuration:

- SSID: `HQ-WIFI`
- Security: WPA2-PSK
- Encryption: AES
- Channel: 6
- VLAN: 90

HQ Wi-Fi clients:

| Client | IP Address | Gateway |
|--------|------------|---------|
| HQ-WIFI-LAP01 | 10.10.90.21 | 10.10.90.1 |
| HQ-WIFI-LAP02 | 10.10.90.22 | 10.10.90.1 |

Both clients successfully associated with AP-HQ and received valid DHCP addresses.

Gateway testing and Internet testing were completed successfully after routing and NAT corrections.

---

# Branch Wi-Fi

AP-BR provides Branch wireless access.

Wireless configuration:

- SSID: `BR-WIFI`
- Security: WPA2-PSK
- Encryption: AES
- Channel: 11
- VLAN: 90

AP-BR wired uplink:

`SW-BR2 Fa0/10`

Final port configuration:

`switchport mode access`

`switchport access vlan 90`

`no shutdown`

Branch Wi-Fi clients:

| Client | IP Address | Gateway |
|--------|------------|---------|
| BR-WIFI-01 | 10.20.90.23 | 10.20.90.1 |
| BR-WIFI-02 | 10.20.90.22 | 10.20.90.1 |

Both clients successfully reached:

`10.20.90.1`

and:

`8.8.8.8`

---

# IoT Wireless Network

AP-IOT provides the dedicated IoT wireless service.

Wireless configuration:

- SSID: `HQ-IOT`
- Security: WPA2-PSK
- Encryption: AES
- Channel: 6
- VLAN: 80

AP-IOT is connected to:

`SW-CORE Fa0/9`

Fa0/9 belongs to VLAN 80.

IoT endpoints:

| Device | IP Address | Gateway |
|--------|------------|---------|
| IOT-TEMP01 | 10.10.80.22 | 10.10.80.1 |
| IOT-MOTION01 | 10.10.80.23 | 10.10.80.1 |
| IOT-DOOR01 | 10.10.80.24 | 10.10.80.1 |

The Packet Tracer IoT devices expose IPv4 information directly through:

`Config → Wireless0`

rather than the Desktop/IP Configuration workflow used by PCs and laptops.

---

# Wireless Security Standard

The project uses WPA2-PSK and AES across all wireless networks.

| Access Point | SSID | Purpose | Security | Channel | VLAN |
|--------------|------|---------|----------|---------|------|
| AP-HQ | HQ-WIFI | HQ user wireless | WPA2-PSK / AES | 6 | 90 |
| AP-BR | BR-WIFI | Branch wireless | WPA2-PSK / AES | 11 | 90 |
| AP-IOT | HQ-IOT | HQ IoT network | WPA2-PSK / AES | 6 | 80 |

The main logical security separation is:

`User Wi-Fi → VLAN 90`

`IoT → VLAN 80`

---

# R-HQ Configuration

R-HQ connects the HQ core to R-ISP.

Interfaces:

`G0/0 = 10.10.254.1/30`

`G0/1 = 10.10.253.1/30`

Default route:

`ip route 0.0.0.0 0.0.0.0 10.10.253.2`

R-HQ uses OSPF to exchange internal routes while the static default provides the Internet exit.

---

# R-ISP NAT/PAT

R-ISP is the NAT/PAT boundary.

Inside interfaces:

`G0/0 = 10.10.253.2/30`

`G0/1 = 10.10.252.2/30`

Outside interface:

`G0/2 = 10.10.255.1/30`

NAT inside is configured on G0/0 and G0/1.

NAT outside is configured on G0/2.

Source ACL:

- 10.20.0.0/16
- 10.10.0.0/16

PAT configuration uses:

`ip nat inside source list 1 interface GigabitEthernet0/2 overload`

Default route:

`ip route 0.0.0.0 0.0.0.0 10.10.255.2`

---

# NAT/PAT Validation

Verification commands:

`show ip nat translations`

`show ip nat statistics`

NAT translations were observed after generating client traffic.

The inside-global address used by R-ISP is:

`10.10.255.1`

Client traffic from both HQ and Branch enterprise networks was translated through PAT toward the simulated Internet endpoint.

The NAT table becomes most useful when traffic is generated first and translations are inspected immediately afterward.

---

# R-INTERNET

R-INTERNET provides a self-contained Internet simulation.

Interface:

`G0/0 = 10.10.255.2/30`

Loopback:

`8.8.8.8/32`

The Loopback0 endpoint acts as the stable Internet test destination.

Validation included:

`R-ISP → 10.10.255.2`

`R-ISP → 8.8.8.8`

`Client → 8.8.8.8`

The simulated endpoint validates IP reachability, routing, PAT and return traffic.

It does not represent a real public Internet connection.

---

# Internet Testing

The full Internet path is:

Client → Local Gateway → Enterprise Routing → R-ISP → R-INTERNET → 8.8.8.8

Gateway testing isolates local VLAN and Layer-2/Layer-3 problems.

The `8.8.8.8` test validates:

- Routing
- Default routes
- NAT/PAT
- Return traffic

A successful ping to `8.8.8.8` should be interpreted as successful reachability to the simulated Internet endpoint.

---

# Troubleshooting Record – Branch DHCP

One of the major troubleshooting incidents involved branch DHCP.

The client initially showed an APIPA address:

`169.254.x.x`

The switch produced DHCP Snooping rate-limit messages.

Observed evidence included:

`DHCP_SNOOPING_ERRDISABLE_WARNING`

and:

`ERR_DISABLE: dhcp-rate-limit error detected on Fa0/4`

Fa0/4 entered an err-disabled state.

The issue was investigated from the endpoint toward the switch and router rather than repeatedly rebuilding the DHCP pool.

After the security configuration was stabilized, the interface remained operational and a branch client received:

`10.20.90.41`

The incident demonstrated that DHCP failure can be caused by a switch security mechanism rather than by the DHCP server itself.

---

# Troubleshooting Record – AP-BR VLAN

Branch wireless initially showed APIPA addresses even though the wireless profile and DHCP service were configured.

The AP-BR uplink was discovered in:

`VLAN 1`

instead of:

`VLAN 90`

The required configuration on SW-BR2 Fa0/10 was:

`switchport mode access`

`switchport access vlan 90`

`no shutdown`

After the correction:

`BR-WIFI-01 → 10.20.90.23`

`BR-WIFI-02 → 10.20.90.22`

Both clients successfully reached their gateway and the simulated Internet.

---

# Troubleshooting Record – HQ Internet

HQ Wi-Fi clients could reach:

`10.10.90.1`

but initially could not reach:

`8.8.8.8`

The investigation identified a missing default-route chain and incomplete NAT source coverage.

Required path:

HQ Client → SW-CORE → R-HQ → R-ISP → R-INTERNET → 8.8.8.8

SW-CORE default:

`ip route 0.0.0.0 0.0.0.0 10.10.254.1`

R-HQ default:

`ip route 0.0.0.0 0.0.0.0 10.10.253.2`

R-ISP NAT source coverage included:

`10.10.0.0/16`

After these dependencies were corrected, HQ Internet testing succeeded and NAT translations appeared on R-ISP.

---

# Server and DNS Considerations

VLAN 70 is reserved for:

`SERVER-DMZ`

The topology includes:

`SRV-DNS2`

The project uses `8.8.8.8` as a configured DNS value for the lab.

However, the R-INTERNET router primarily provides an ICMP test endpoint.

Therefore:

`ping 8.8.8.8`

validates IP reachability.

It does not guarantee:

`ping google.com`

or public DNS resolution.

A production extension could include:

- Internal DNS
- Centralized services
- DNS ACL policies
- DHCP options pointing to the internal resolver

---

# Management Plane

Management VLANs:

HQ:

`10.10.99.0/24`

Branch:

`10.20.99.0/24`

Management controls include:

- SSH Version 2
- Local administrator account
- Privilege 15
- Domain `corp.local`
- Login banner
- Dedicated management subnet

Future management enhancements could include:

- TACACS+
- RADIUS
- NTP
- Syslog
- SNMP
- Management ACLs

---

# Endpoint Validation

## HQ Wi-Fi

HQ-WIFI-LAP01:

`10.10.90.21`

Gateway:

`10.10.90.1`

Internet:

PASS

HQ-WIFI-LAP02:

`10.10.90.22`

Gateway:

`10.10.90.1`

Internet:

PASS

---

## Branch Wi-Fi

BR-WIFI-01:

`10.20.90.23`

Gateway:

`10.20.90.1`

Internet:

PASS

BR-WIFI-02:

`10.20.90.22`

Gateway:

`10.20.90.1`

Internet:

PASS

---

## IoT

IOT-TEMP01:

`10.10.80.22`

Gateway:

`10.10.80.1`

IOT-MOTION01:

`10.10.80.23`

Gateway:

`10.10.80.1`

IOT-DOOR01:

`10.10.80.24`

Gateway:

`10.10.80.1`

---

# Final Verification Matrix

| Area | Verification | Expected Result | Status |
|------|--------------|-----------------|--------|
| VLAN | show vlan brief | Correct access membership | PASS |
| Trunk | show interfaces trunk | Required VLANs allowed/forwarding | PASS |
| LACP | show etherchannel summary | Port-channel4 active | PASS |
| OSPF | show ip ospf neighbor | FULL adjacency | PASS |
| DHCP | show ip dhcp binding | Client leases present | PASS |
| Guest ACL | Guest to internal HQ network | Blocked | PASS |
| HQ Wi-Fi | Client addressing | 10.10.90.x | PASS |
| Branch Wi-Fi | Client addressing | 10.20.90.x | PASS |
| IoT | Device addressing | 10.10.80.x | PASS |
| NAT | show ip nat translations | Dynamic mappings | PASS |
| Internet | ping 8.8.8.8 | Replies | PASS |

---

# Packet Tracer Command Compatibility

Command syntax can vary between Cisco IOS images and Packet Tracer device models.

During this project, some command variants commonly found in Cisco documentation were rejected by the Packet Tracer image.

The final project therefore uses syntax that was actually accepted and validated during implementation.

Useful verified commands include:

- `show running-config`
- `show interfaces status`
- `show vlan brief`
- `show interfaces trunk`
- `show mac address-table`
- `show etherchannel summary`
- `show ip interface brief`
- `show ip route`
- `show ip ospf neighbor`
- `show ip dhcp binding`
- `show ip dhcp snooping`
- `show ip arp inspection`
- `show ip nat translations`
- `show ip nat statistics`

---

# Recommended Troubleshooting Workflow

Use this order when diagnosing a problem:

1. Check physical/interface state.
2. Check VLAN membership.
3. Check trunk forwarding.
4. Check EtherChannel state.
5. Check the local gateway.
6. Check OSPF adjacency.
7. Check DHCP.
8. Check the default route.
9. Check NAT.
10. Test the Internet endpoint.

For wireless:

1. Verify wireless adapter.
2. Verify SSID.
3. Verify authentication.
4. Verify AP uplink VLAN.
5. Verify DHCP.
6. Verify gateway.
7. Verify Internet.

For IoT:

1. Open Config.
2. Select Wireless0.
3. Verify SSID.
4. Verify WPA2-PSK.
5. Verify AES.
6. Verify DHCP.
7. Verify 10.10.80.x addressing.

---

# Backup and Recovery

The final Packet Tracer `.pkt` file is the main recovery artifact.

After important configuration changes:

`write memory`

The Packet Tracer project should also be saved using:

`Ctrl + S`

Recommended project artifacts include:

- Final `.pkt` file
- Complete PDF documentation
- Configuration files
- README
- Supporting screenshots when available

Recovery process:

1. Open the last known-good `.pkt` file.
2. Verify interface state.
3. Verify VLANs.
4. Verify trunks.
5. Verify EtherChannel.
6. Verify OSPF.
7. Verify DHCP.
8. Verify NAT.
9. Repeat the final verification matrix.

---

# Project Lessons Learned

## Lesson 1 – DHCP problems are not always DHCP server problems

A client with a `169.254.x.x` address may be affected by:

- Wrong VLAN
- Failed trunk
- Err-disabled switchport
- DHCP Snooping
- Incorrect router subinterface
- DHCP service configuration

The project demonstrated this during branch troubleshooting.

## Lesson 2 – Wireless depends on the wired VLAN

An AP can have the correct SSID and security profile while still failing to provide DHCP if its switchport is assigned to the wrong VLAN.

## Lesson 3 – OSPF and Internet defaults solve different problems

OSPF provides internal enterprise route learning.

Static default routes provide the path for destinations outside the known enterprise networks.

## Lesson 4 – NAT must include the correct source networks

Branch traffic uses the `10.20.0.0/16` range.

HQ traffic uses the `10.10.0.0/16` range.

Both ranges therefore need to be eligible for PAT.

## Lesson 5 – Configuration must be validated by behavior

A configured DHCP pool does not prove that DHCP is working.

A configured NAT rule does not prove that traffic is being translated.

A configured OSPF process does not prove that the adjacency is FULL.

The project therefore uses configuration verification and behavioral testing together.

---

# Future Enhancements

The project can be extended with:

- Centralized AAA
- TACACS+
- RADIUS
- Syslog
- NTP
- SNMP
- Network monitoring
- Advanced ACL policies
- Redundant core switches
- Redundant WAN links
- HSRP/VRRP
- IPv6
- QoS
- Centralized DNS
- Network automation
- IDS/IPS integration
- SIEM integration
- Advanced wireless controller architecture

---

# Project Limitations

This project is implemented as a Cisco Packet Tracer simulation.

Therefore:

- Internet connectivity is simulated.
- `8.8.8.8` is a lab endpoint.
- Public DNS resolution is not guaranteed.
- DHCP Snooping on SW-BR2 includes a Packet Tracer compatibility workaround.
- Wireless RF behavior is simplified.
- Centralized AAA was outside the project scope.
- Hardware performance is simulated.

---

# Repository Structure

enterprise-network-cisco-packet-tracer/

├── Packet-Tracer/

│   └── realistic enterprise network.pkt

├── Documentation/

│   └── Cisco_Packet_Tracer_Project_Documentation_With_Cover (1).pdf

├── Screenshots/

├── Configurations/

├── README.md

└── LICENSE

---

# Documentation

The complete project documentation is available in the `Documentation` folder.

The documentation covers:

- Project objectives
- Network architecture
- VLAN design
- IP addressing
- SW-CORE
- SW-BR1
- SW-BR2
- EtherChannel/LACP
- Spanning Tree
- OSPF
- DHCP
- ACL
- SSH
- Port Security
- DHCP Snooping
- Dynamic ARP Inspection
- HQ Wi-Fi
- Branch Wi-Fi
- IoT
- NAT/PAT
- Internet simulation
- Troubleshooting
- Validation
- Backup and recovery
- Final acceptance

---

# Project Information

## Project Title

Enterprise Network Infrastructure Using Cisco Packet Tracer

## Domain

Computer Networking

## Project Type

Academic / Training Project

## Institution

BIGLEARN TRAINING INSTITUTE, TIRUCHIRAPPALLI

## Guided By

SYED KHAJA S A

## Developed By

VISHAL B

---

# Final Project Status

The final enterprise network implementation provides:

- HQ VLAN segmentation
- Branch VLAN segmentation
- Inter-VLAN routing
- Router-on-a-stick
- OSPF dynamic routing
- Branch DHCP
- Guest ACL isolation
- SSH Version 2
- EtherChannel / LACP
- PVST
- Port Security
- PortFast
- BPDU Guard
- DHCP Snooping with documented Packet Tracer exception
- Dynamic ARP Inspection
- HQ Wi-Fi
- Branch Wi-Fi
- Dedicated IoT Wi-Fi
- NAT/PAT
- Simulated Internet
- End-to-end connectivity validation

The project was validated using gateway reachability, routing tables, OSPF neighbor state, DHCP lease information, wireless client addressing, NAT translations and reachability to the simulated `8.8.8.8` Internet endpoint.

---

# Final Acceptance

The project is considered complete when:

- The final `.pkt` file is saved.
- VLAN configuration is correct.
- Trunks are forwarding required VLANs.
- EtherChannel is operational.
- OSPF adjacency is established.
- DHCP leases are available.
- Guest ACL isolation works.
- HQ Wi-Fi clients receive 10.10.90.x addresses.
- Branch Wi-Fi clients receive 10.20.90.x addresses.
- IoT devices receive 10.10.80.x addresses.
- NAT translations appear after traffic generation.
- The simulated Internet endpoint `8.8.8.8` is reachable.

The completed project demonstrates practical enterprise networking implementation, validation and troubleshooting using Cisco Packet Tracer.

---

# Author

Vishal B

## Institution

BIGLEARN TRAINING INSTITUTE, TIRUCHIRAPPALLI

## Guided By

SYED KHAJA S A

## Field

Computer Networking

## Project

Enterprise Network Infrastructure Using Cisco Packet Tracer
