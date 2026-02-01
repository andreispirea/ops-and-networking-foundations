# IP Plan 

## Overview 
- Lab Name: ops-and-networking-foundations
- Purpose: simulate a small enterprise LAN with internal services and internet access via NAT on the client

## Networks 
### NAT Network (Internet)
- Virtualization mode: NAT (VirtualBox)
- Purpose: Windows outbound internet access for testing
- Addressing: DHCP from hypervisor (default)

### Host-only Network (Internal LAN)
- Virtualization mode: Host-only
- Subnet/CIDR: 192.168.56.0/24
- Purpose: internal connectivity between Windows and Ubuntu
- DHCP: Disabled

## Hosts
### Windows 11 (Client)
- Hostname: LAB-W1-WIN11
- NIC1 (NAT): NATNetwork-Lab
  - IP: (DHCP)
  - DNS: 
  - Default gateway: (DHCP)
- NIC2 (Host-only): VirtualBox Host-Only Enthernet Adapter
  - IP: 192.168.56.10
  - Subnet mask: 255.255.255.0
  - Default gateway: none
  - DNS: none

### Ubuntu Server (Internal service host)
- Hostname:
- NIC1 (Host-only):
  - IP: 196.168.56.20
  - CIDR:
  - Default gateway: none
  - DNS: ?

## Notes / Design choices
- Which NIC should Windows prefer for internet? (NAT)
- Expected connectivity:
  - Windows ↔ Ubuntu over Host-only: Yes
  - Ubuntu → Internet: Not required 
