# DHCP Snooping #
-   By enabling DHCP snooping, you are telling the switch to ignore all ports that are not explicitly trusted. We tell the switch what specific port leads to the real DHCP server.

![[dhcp-snooping.png]]

- ```ip dhcp snooping``` Enables dhcp snooping.
- ```sh dhcp snooping```
- ```ip dhcp snooping vlan 1```
- ```sh dhcp snooping ```
- ```no ip dhcp snooping information option``` This disables option 82. To enable DHCP snooping on a trusted interface. Go into the interface: conf t then the interface: int fa0/1. Then enable that port as a trusted DHCP port.
- ```ip dhcp snooping trust``` That's now a trusted DHCP port.
- ```sh dhcp snooping binding``` To see leases.

### Source Guard ###
- ```ip verify source port security```This verifies both the source ip and MAC address.
- ```show ip verify source```