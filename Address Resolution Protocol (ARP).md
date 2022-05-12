# Address Resolution Protocol (ARP)
 ARP is a communication protocol used for discovering the link layer address, such as a MAC address, associated with a given internet layer address, typically an IPv4 address. This mapping is a critical function in the Internet protocol suite.

## ARP Probe:
 ARP probe is an ARP request constructed with an all-zero SPA. Before beginning to use an IPv4 address (whether received from manual configuration, DHCP, or some other means), a host implementing this specification must test to see if the address is already in use, by broadcasting ARP probe packets.

 *Note: This is usually sent during the DCHP process to verify that the ip address is in fact open.*

## ARP Announcments:

 ARP may also be used as a simple announcement protocol. This is useful for updating other hosts' mappings of a hardware address when the sender's IP address or MAC address changes. Such an announcement, also called a **gratuitous ARP (GARP) message**, is usually broadcast as an ARP request containing the SPA in the target field (TPA=SPA), with THA set to zero. An alternative way is to broadcast an ARP reply with the sender's SHA and SPA duplicated in the target fields (TPA=SPA, THA=SHA).

- TPA: Target Protocol Address
- THA: Target Hardware Address
- SPA: Sender Protocol Address
- SHA: Sender Hardware Address

The ARP request and ARP reply announcements are both standards-based methods, but the ARP request method is preferred. Some devices may be configured for the use of either of these two types of announcements.

An ARP announcement is not intended to solicit a reply; instead, it updates any cached entries in the ARP tables of other hosts that receive the packet. The operation code in the announcement may be either request or reply; the ARP standard specifies that the opcode is only processed after the ARP table has been updated from the address fields.

Many operating systems issue an ARP announcement during startup. This helps to resolve problems which would otherwise occur if, for example, a network card was recently changed (changing the IP-address-to-MAC-address mapping) and other hosts still have the old mapping in their ARP caches.

ARP announcements are also used by some network interfaces to provide load balancing for incoming traffic. In a team of network cards, it is used to announce a different MAC address within the team that should receive incoming packets.

ARP announcements can be used in the Zeroconf protocol to allow automatic assignment of a link-local IP addresses to an interface where no other IP address configuration is available. The announcements are used to ensure an address chosen by a host is not in use by other hosts on the network link.

This function can be dangerous from a cybersecurity viewpoint since an attacker can obtain information about the other hosts of its subnet to save in their ARP cache (ARP spoofing) an entry where the attacker MAC is associated, for instance, to the IP of the default gateway, thus allowing him to intercept all the traffic to external networks.

# ARP spoofing and proxy ARP:
Because ARP does not provide methods for authenticating ARP replies on a network, ARP replies can come from systems other than the one with the required Layer 2 address. An ARP proxy is a system that answers the ARP request on behalf of another system for which it will forward traffic, normally as a part of the network's design, such as for a dialup internet service. By contrast, in ARP spoofing the answering system, or spoofer, replies to a request for another system's address with the aim of intercepting data bound for that system. A malicious user may use ARP spoofing to perform a man-in-the-middle or denial-of-service attack on other users on the network. Various software exists to both detect and perform ARP spoofing attacks, though ARP itself does not provide any methods of protection from such attacks.

*Note: IPv6 uses the Neighbor Discovery Protocol and its extensions such as Secure Neighbor Discovery, rather than ARP.*
