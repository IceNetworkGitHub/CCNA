# ACLs For Packet Filtering #
- An ACL identifies traffic based on characteristics of the packet such as source IP address, destination IP address, port number.
- ACLs are supported on both routers and switches.
- ACLs applied to an interface do not apply to traffic which originates from the router itself.
- An ACL is read from top to bottom.
	- **As soon as a rule matches the packet, the permit or deny action is applied and the ACL is not processed any further.**
	- The order of rules is important.
- The original use of ACLs was a security feature to decide if traffic should be allowed to pass through the router. By default a router will allow all traffic to pass between its interfaces. When ACLs are applied the router identifies traffic and then decides if it will be allowed or not.
- ACLs are also used in other software policies when traffic has to be identified, for example.
	- Identify traffic to give better service to in a QoS Quality of Service Policy.
	- Identify traffic to translate to a different IP address in a NAT Network Address Translation Policy.
- Access Control Lists are made up of Access Control Entries which are a series of permit or deny rules. Each ACE is written in a seperate line. Here is an example.
	- ```access-list 100 deny tcp 10.10.30.0 0.0.0.255 gt 49151 10.10.20.1 0.0.0.0 eq 23```
	- The source number is gt (greater than) 49151. 10.10.30.0 is the source. 10.10.20.1 is the destination.  23 is the port.
- Remember, if this was our list, without the ANY PERMIT we would block all traffic from coming through.

![[aclspacketfiltering.png]]

### Access Groups ###
- ACLs are applied at the interface level with the Access-Group command.
- ACLs can be applied in the inbound or outbound direction.
- You can have a maximum of one ACL per interface per direction.
- You can have both an inbound and an outbound ACL on the same interface, but not 2 inbound or outbound ACLs.
- An interface can have no ACL applied, an inbound ACL only, an outbound ACL only, or ACLs in both directions.
- Done on the interface level. ```ip access-group 100 out``` or ```ip access-group 101 in``` 
- ```show ip interface f1/0 | include access list```

### Standard vs Extended ACLs ###
- With Standard ACLs, all we can match on is the source IP address. For anything else we have to use extended ACL. Standard ACLs are numbered from 1 through 99. 100 through 199 are extended ACLs. We can also use NAMED ACLs which are easier to manage.
- 1300 - 1999 are IP standard access list (expanded range).
- 2000 - 2699 are extended access list (expanded range).
- There is no default wildcard mask for Extended ACLs. You have to specify a wildcard mask.

### Extended ACLs ###
-   First check to see if there are any access lists.
    -   show access-lists
-   Enter global configuration mode.
    -   access-list 100 deny icmp host 10.16.0.10 host 192.168.1.100 log
-   To deny http traffic from a specific host to a specific server.
    -   access-list 100 deny tcp host 10.16.0.10 host 192.168.1.100 eq 80

### Named ACLs ###
- Named ACL starts with the IP command.
-   To create a named access list.
    -   ```ip access-list extended Our-ACL```
-   Now are in access list configuration mode.
    -  ```deny tcp host 10.16.0.10 any eq 21```
-   Another example
    -  ```deny ip 10.16.0.0 0.0.0.255 10.16.16.0 0.0.7.255```
-   Another example
    -  ```deny icmp 10.16.0.0 0.0.0.255 10.16.2.0 0.0.0.255```
-   Remember to allow the rest.
    -  ```permit ip any any```
-   Now we need to apply this named ACL to an interface. Select the interface.
    - ```ip access-group Our-ACL | inbound | outbound```
-   Verify by checking the interface. ```sh ip int``` and look at the inbound and outbound access settings.

### To Create and Verify an ACL ###
-   First is to check to see if there are any access lists already. If there is a list already and we start typing away commands, we are adding to the list that’s already in place, not creating a new one. It's also a good rule to put a remark on an ACL.
    -   ```show access-lists```
-   It’s also good to check the access lists on the interface.
    -   See outgoing access list and inbound access list.
-   Create the list.
    -   ```access list 1 deny host 10.16.0.10 log```
    -   ```access list 1 permit any```
    - There is an implicit 'deny any any' rule at the bottom of ACLs.
    - If an ACL is not applied to an interface, all traffic is allowed.
    - **If an ACL is applied, all traffic is denied except what is explicitly allowed.**
    - Many companies put this statement at the end of the ACL so they log any deny's.
    - ```access-list 1 deny any log```
-   Go to the interface on the router and apply it to the correct interface.
    -   ```ip access-group 1 | inbound | outbound```
- Inject an Access Control Entry into the ACL. You can do it with numbered and named ACLs.
	- ```ip access-list extended 110```
	- ```15 deny tcp host 10.10.10.11 host 10.10.50.10 eq telnet```

### Wildcard Mask With an ACL ###
-   access list 2 deny host 10.16.0.0 0.0.255.255
-   Remember to create permit any for access list 2.




