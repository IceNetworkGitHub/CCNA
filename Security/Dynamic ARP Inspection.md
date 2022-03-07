# Dynamic ARP Inspection #
- Why DAI is important.
	- To know if PC’s are lying about their layer 2 information. A malicious host could spoof the MAC address of another host to get the layer to frames destined for the legitimate host. The malicious host if he is using good software will then forward the frames to the legitimate host once he has ease dropped on the frames. The traffic is going through the man in the middle.
- ```sh ip dhcp snooping binding``` See IP/MAC addresses that are in the dhcp table.

- DAI commands.
	- ```show ip arp inspection``` See basic info.
	- ```show ip arp inspection statistics```
	- ```show ip arp inspection vlan 30``` See per vlan ip arp inspection vlan 60, done in global config mode.
	- ```ip arp inspect trust``` Done on the interface level, towards a router e.g.

- The DAI process can use the binding table from DHCP snooping but may deny some ARP messages because they're from non-DHCP clients and the switch doesn't know the L3/L2 mappings for those clients.
	- ```show ip dhcp snooping``` To see if dhcp snooping is enabled which DAI uses.

- Everything that’s not in the DHCP snooping TABLE will be denied. We need to create static entries for those devices by using ARP ACCESS CONTROL LIST.
	-   This ARP list will have a IP ADDRESS and MAC ADDRESS
	- ```arp access-list OUR-ACCESS-LIST``` This creates a ARP access list using that name //cli drops us down into the OUR-ACCESS-LIST configuration mode.
	- ```permit ip host 10.0.1.111 mac host 1234.5678.9101```

- To enable the ARP inspection list we created.
	- ```ip arp inspection filter OUR-ACCESS-LIST vlan 30``` Done in global config mode.

### Limit ARP traffic on untrusted ports ###
- Which of the following is the default packet per second rate limiting for ARP traffic on an untrusted port? 15.
	- ```show arp inspection interfaces```
-   For instance if somebody does nmap -sn for a network that floods out arp requests and that port will be shutdown or something else according to the policy in place.
    
- ```show ip arp inspection``` This shows us that **source mac validation, destination mac validation, and ip address validation** are all disabled by default.
    
- To enable of any of those 3, go into global config mode
    ```ip arp inspection validate ?```  To see the 3 options.