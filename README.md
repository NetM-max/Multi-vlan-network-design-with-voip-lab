This is a small lab network implementing Layer 2 redundancy, Layer 3 segmentation, and integrated IP telephony services.
The objective was to design and troubleshoot a multi-VLAN architecture supporting:
- Segmented user and voice networks.
- Centralized DHCP services.
- Inter-VLAN routing on a Layer 3 core.
- EtherChannel uplinks.
- Rapid-PVST spanning tree optimization.
- Cisco Call Manager Express (CME) for VoIP call control.
The topology follows a hierarchical design model (Core–Distribution–Access).

Network Topology:

Core Layer:
- Core1 (Layer 3 Switch). Function: Inter-VLAN routing using SVIs, STP Root Bridge (Rapid-PVST), Default gateways for all VLANs, DHCP relay (ip helper-address), EtherChanell with PaGP.
- Destribution 1 and 2. (2 L3 Switches)
- Access (2 Switches, different floors). Providing access to the end devices.
  
VLAN Design

- VLAN 10: HR, Subnet: 192.168.0.0/23, Default Gateway: 192.168.0.1/23.
- VLAN 20: Management, Subnet: 192.168.2.0/23, Default Gateway: 192.168.2.1/23.
- VLAN 30: Phones, Subnet: 192.168.4.0/23, Default Gateway 192.168.4.1/23.
- VLAN 40: Users, Subnet: 192.168.6.0/23, Default Gateway 192.168.6.1/23.
- VLAN 50: Servers, Subnet: 192.168.8.0/23, Default Gateway 192.168.8.1
- VLAN 99: Native.
Inter-VLAN routing is performed entirely on the Core Layer 3 switch using SVIs.

Spanning Tree Design:
- STP Mode: Rapid-PVST.
- Root Bridge: Core1.
Core1 was manually configured as root to ensure deterministic forwarding paths, prevent suboptimal blocking, maintain predictable failover behavior.

EtherChannel Implementation:
- Uplinks between Core and Distribution switches are bundled using PAgP.

DHCP:
- DHCP Server Location: Behind Distribution layer (Server VLAN). 
- Centralized DHCP model. 
- ip helper-address was configured on each SVI on the Core switch.

VoIP:
-Voice VLAN: 30
-CME Router IP : 192.168.4.5/23

Troubleshooting Scenarios Resolved:
- Incorrect DHCP Option 150 preventing phone registration. For this one I did not add correct TFTP Server Address on DHCP VLAN 30 (voice) pool. I added the default SVI gateway but I should have added the IP interface of the CME Router.
- Did not add ip-helper address on each SVI causing DHCP to fail. 
