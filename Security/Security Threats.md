# Security Threat Landscape #

## The Security Threat Landscape ##

- **Threat:** has the potential to cause harm to an IT asset.
- **Vulnerability:** a weakness that compromises the security or funtionality of a system.
- **Exploit:** uses a weakness to compromise the security or funtionality of a system.
- **Risk:** the likelihood of a successful attack.
- **Mitigation:** techniques to eliminate or reduce the potential of and seriousness of an attack.
- **Malware:** is malicious software, including:
	- Viruses: software which inserts itself into other software and can spread from computer to computer. Requires human action to spread.
	- Worms: a self-propagating virus that can replicate itself.
	- Trojan horses: malicious software which looks legitimate to trick humans into triggering it. Often installs back doors.
	- Ransomware: Encrypt data with the attacker's key and asks the victim to pay a ransom to obtain the key.

## Common Attacks ##
- TCP Syn Flood Attack.
- DDos, A distributed denial of service.
- Reflection and Amplification Attack.
- Man In The Middle Attacks such as ARP spoofing.
- Password Attacks.
- Malware.

| Attack               | Counter Measures                                                                       |
|----------------------|----------------------------------------------------------------------------------------|
| VLAN Hopping         | Static access ports, disabling of DTP, avoidance of trunk native VLAN on access ports. |
| STP Spoofing         | BDPU Guard/Root Guard                                                                  |
| CAM/MAC Spoofing     | Port Security (MAC LIMIT)                                                              |
| ARP Spoofing         | ARP inspection                                                                         |
| DHCP Starvation      | Port Security                                                                          |
| DHCP Server Spoofing | DHCP snooping                                                                          |

## Firewalls and IDS/IPS ##
- IDS: Intrusion detection system.
- IPS: Intrusion prevention system.
- IDS and IPS use signatures to inspect packets up to layer 7 of the OSI stack, looking for traffic patterns which match known attacks.
- They can also use anomaly-based inspection to look for unusual behaviour, such as a host sending more traffic than usual.
- They require skilled staff to tune the IPS to their own particualar environment and minimize false postitives and negatives.
- IDS sits alongside the traffic flow and informs security administrators of any potential concerns.
- IPS sits inline with the traffic flow and can also block attacks.
- An IDS may also have the capability to tell a firewall to block attacks.

## Firewalls vs Packet Filters ##
- Firewalls secure traffic passing through them by either permitting or denying it.
- Stateful firewalls maintain a connection table which tracks the two-way 'state' of traffic passing through the firewall.
- Return traffic is permitted by default.
- Firewall rules example:
	- Deny all traffic from outside to inside.
	- Permit outbound we traffic from 10.10.10.0/24.
- Next Generation Firewalls move beyond port/protocol inspection and blocking to add application-level inspection, intrusion prevention, and user based security.
- Deep packet inspection analyses packets up to layer 7 of the OSI stack.
- Different permissions can be applied to different users.
- The Cisco ASA with FirePower is a Next Generation Firewall.

## Cryptography ##
- Cryptography provides these services to data:
	- Authenticity (proof of source).
	- Confidentiality (privacy and secrecy).
	- Integrity (has not changed in transit).
	- Non-repudiation (non-deniability).

### Symmetric Encryption ###
- With symmetric encryption, the same shared key both encrypts and decrypts the data.
- The shared key is know by both the sender and receiver and must be kept secret.
- Fast.
- Used for large transmissions (eg., email, secure web traffic, IPsec).
- Algorithms include DES, 3DES, AES, SEAL.

### Asymmetric Encryption ###
- Asymmetric encryption uses private and public key pairs.
- Data encrypted with the public key can only be decrypted with the private key, and vice versa.
- Data encrypted with the public key **cannot** be decrypted with the public key.
- Only the private key must be kept secret.
- The public key can be available in the public domain.
- It's slow.
- Used for small transmission (symmetric key exchange, digital signatures).
- Algorithms include: RSA, ECDSA.

### Transport Layer Security TLS ###
- SSL: Secure Socket Layer (Deprecated).
- TLS: Transport Layer Security (seccessor to SSL).
- Can be used to provide secure web browsing with HTTP (can also be used with other applications such as email).
- Uses symmetric cryptography to encrypt transmitted data.
- Symmetric keys are generated uniquely for each connection.
- Authentication is provided by public key cryptography.
- Message Authentication Code provides integrity.

## VPN ##
### Site-to-Site ###
- ```crypto isakmp policy 1``` Done in global configuration.
- ```encr aes```
- ```hash sha```
- ```authentication pre-share```
- ```group 2```
- ```lifetime 86400```
- ```crypto isakmp key Flackbox address 203.0.113.5```
- ```ip access-list extended FlackboxVPN-ACL``` Done in global configuration.
- ```permit ip 10.10.10.0 0.0.0.255 10.10.20.0 0.0.0.255```

### Remote Access VPN ###

## Threat Defense Solutions ##
- 
- 