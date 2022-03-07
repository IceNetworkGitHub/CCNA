# Port Security #
- ```show port-security```
- ```show port-security int gig/0/0 ```
- ```show int status err-disabled``` To see disabled ports.
- ```switchport port-security``` This enables the port-security rules we set for the interface.

-   Check first to see how many MAC addresses are behind a port before turning on port-security. If we see more than one, the port will be shutdown. Investigate first.
- Best practice is to administratively shut down unused switch ports. This stops somebody getting access to the network if they physically connect to the port.
-   Cam table overflow is used to send thousands of frames from thousands of bogus MAC addresses. When his CAM table is consumed and full, the switch is not able to remember MAC addresses. It stops knowing which port has what MAC address associated with it and the switch starts sending out traffic on all the ports and the attacker can listen in on the traffic regardless of the VLAN it’s associated with.
-   Ports cannot be dynamic if we want to setup port security on. It cannot be a dynamic layer 2 port. Fix that, switchport mode access or trunk. Another trick is to do switchport host, that forces the port into access mode and it also turns on portfast for that port, then the port doesn’t have to wait for the listening and learning states for spanning tree.
-   To enable port security on that interface.
    -   ```switchport security```
-   Turning on port security forces the interface to **only allow one MAC address** and it's not locked down to a particular MAC address.
-   The default response to violating port security turns off the port.

![[switchportsecurity.png]]

-   To change the maximum of MAC addresses for an interface.
    -   ```switchport port-security maximum 5```
-   We can also hard code MAC addresses.
    -   ```switchport port-security mac-address 00H.2233.4455```
-   Another way is to use sticky. It takes the MAC addresses learned dynamically and puts it into the running config. This is the scaleable solution. Hardcoding the MAC addressing in only works in really small networks. If we want to make them permanent we would save the startup config.
    -  ```switchport port-security mac-address sticky```
-   We can change the default violation response.
    -  ```switchport port-security violation | restrict | protect | shutdown```
        - **Shutdown (Default):** The interface is place into the error-disabled state, blocking all traffic.
        - **Protect:** Traffic from unauthorised addresses is dropped. Traffic from allowed addresses is forwarded.
        - **Restrict:** Traffic from unauthorised addresses is dropped, logged and the violation counter incremented. Traffic from allowed addresses is forwarded. 
	        - Restrict, restrict the number to the set maximum number, protect does the same but restrict has additional options such as logging capabilities and it increments the port security hit counter for violations.
-   To enable all the port-security rules we created for an interface we need to enable them by typing in.
    -   ```switchport port-security```

### Automate the switch response to an err-disabled port

-   ```show errdisable recovery```
    -   Look for psecure-violation
- ```errdisable recovery cause psecure-violation``` Done in global configuration.
    -   ```errdisable recovery interval 30``` Changes it to 30 seconds. The default is 5 minutes.