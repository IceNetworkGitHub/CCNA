# NAT Network Address Translation #
- ```show ip nat translations```
	- Inside Global: The NAT'd address of the inside host as it will be reached by the outside network.
	- Inside Local: The IP address actually configured on the inside host's Operating system.
	- Outside Local: The IP address of the outside host as it appears to the inside network.
	- Outside Global: The IP address assigned to the host on the outside network by the host's owner.

	-   **How to describe NAT, correct terms. The four terms are.**
    -   Inside - (Who owns that address, YOU).
    -   Outside - Who owns that address, SOMEONE ELSE).
    -   Local - Private
    -   Global - Public

- For one way NAT, the Outside Local and Outside Global addresses will be reported as being the same.
- Two way NAT is not a part of the CCAN, study later.

- RFC 1918 Private addresses. The internet Engineering Task Force (IETF) documents standards with RFC's (Requests For Comments).
- RFC 1918 specifies private IP address ranges which are not routable on the public internet.
- Private addresses were originally designed for hosts which should have no internet connectivity.

## Three Versions of NAT ##
1.  **Dynamic NAT (Overload)**
- Dynamic NAT - uses a pool of public addresses which are given out on an as needed first come first served basis. Usually used for internal hosts which need to connect to the Internet **but do not accept incoming connections** that's not initiated by them. They do accept return traffic that is initiated by them. 
- With the standard Dynamic NAT you need a public IP address for every inside host which needs to communicate with the outside.
- If you have 30 hosts, you need 30 public IP addresses. When all the addresses in the pool have been used, new outbound connections from other inside hosts will fail becauce there will no addresses left to translate them to. These hosts would have to wait for existing connections to be torn down and the translations to be released back into the pool when they time out.
-   The three steps to configure Dyanmic NAT are.
-   Configure an ACL.
   -   Identify interfaces - Inside/Outside.
	-   ```ip nat outside``` The interface that’s going to be doing the NATTING. Outside facing interface.
	-   ```ip nat inside``` The inside facing interface.
	- Configure the pool of global addresses.
		- ```ip nat pool Flackbox 203.0.113.4 203.0.113.14 netmask 25.255.255.240```
	- Create an access list which references the internal IP addresses we want to translate.
		- ```access-list 1 permit 10.0.2.0 0.0.0.255```
	- Associate the access list with the NAT pool to complete the configuration.
		- ```ip nat inside source list 1 pool Flackbox```

- ```show ip nat translation```
- ```show ip nat statistics```
- ```clear ip nat translations``` Can be used to remove translations from the translation table. This can be useful when troubleshooting. It is also often required if you want to edit your NAT configuration - the router will not allow changes when there are active translations.
- ```clear ip nat translations *``` will remove all dynamic translations.

3.  **Static NAT**
-   IP NAT inside source static 10.16.0.200 203.0.113.3
- Static NAT - permanent one-to-one mapping usually between a public and private IP address. Used for servers which must accept incoming connections.
- **Configuration**
	- ```int f0/0```
		- ```ip nat outside```
	- ```int f1/0```
		- ip nat inside
	- ```ip nat inside source static 10.0.1.10 203.0.113.3```

4.  **PAT Port Address Translation (NAT Overload)**
-   NAT Overload, also known as PAT which stands for PORT ADDRESS TRANSLATION instead of NAT, NETWORK ADDRESS TRANSLATION.
-   A combination of IP address and port number is known as a SOCKET. 203.0.113.100:11100
-   How to describe NAT, correct terms. The four terms are.
    -   Inside - (Who owns that address, YOU).
    -   Outside - Who owns that address, SOMEONE ELSE).
    -   Local - Private
    -   Global - Public
- **Configuration**
	- ```int f0/0```
		- ```ip nat outside```
	- ```int f1/0```
		- ```ip nat inside```
	- ```ip nat inside source static 10.0.1.10 203.0.113.3 overload``` The only difference is specifying overload!

- This configuration uses a pool of outside addresses. If you it's a small office with one outside IP address you do the following.
	- ```access-list 1 permit 10.0.2.0 0.0.0.255```
	- ```ip nat inside source list 1 interface f0/0 overload``` This maps it to the interface, not to a pool of IP addresses.

![[pat.png]]
