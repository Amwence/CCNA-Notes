# Network Address Translation:

Network Address Translation-Network address translation (NAT) is a method of mapping an IP address space into another by modifying network address information in the IP header of packets while they are in transit across a traffic routing device. The technique was originally used to avoid the need to assign a new address to every host when a network was moved, or when the upstream Internet service provider was replaced, but could not route the networks address space. It has become a popular and essential tool in conserving global address space in the face of IPv4 address exhaustion. One Internet-routable IP address of a NAT gateway can be used for an entire private network. Most generally used to translate private ip addresses to publicly routable ip addresses to get network traffic onto the internet. 

# Port Address Translation:

Port Address Translation- All IP packets have a source IP address and a destination IP address. Typically packets passing from the private network to the public network will have their source address modified, while packets passing from the public network back to the private network will have their destination address modified. To avoid ambiguity in how replies are translated, further modifications to the packets are required. The vast bulk of Internet traffic uses Transmission Control Protocol (TCP) or User Datagram Protocol (UDP). For these protocols the port numbers are changed so that the combination of IP address (within the IP header) and port number (within the Transport Layer header) on the returned packet can be unambiguously mapped to the corresponding private network destination. RFC 2663 uses the term network address and port translation (NAPT) for this type of NAT. Other names include port address translation (PAT), IP masquerading, NAT overload and many-to-one NAT. This is the most common type of NAT and has become synonymous with the term "NAT" in common usage.

This method enables communication through the router only when the conversation originates in the private network since the initial originating transmission is what establishes the required information in the translation tables. A web browser in the masqueraded network can, for example, browse a website outside, but a web browser outside cannot browse a website hosted within the masqueraded network. Protocols not based on TCP and UDP require other translation techniques.

One of the additional benefits of one-to-many NAT is that it is a practical solution to IPv4 address exhaustion. Even large networks can be connected to the Internet using a single public IP address. NAT OVERLOADING.

Inside user or inside hosts, servers you connect to over the internet would be outsider and on the Global internet. The pc on the inside of your network would be inside your local network. The internet on the other hand is global, and the server we mentioned earlier would be on the global internet outside of your network.

# Terminology:

**Inside Local Address**- private address of the inside host on the local lan

**Inside Global Address**- is the address of the local inside pc as seen on the global internet with a Public IP address. (Natted Address)

**Outside Local Address**- is the address of the outside server as seen on the Local Lan. Outside local address is the real IP address of the end device at other network. 

***Note: Outside local addresses are typically private IP addresses assigned to the computers in the another private network. We cannot know the Outside local addresses because in a NAT enabled network we use the destination IP address as Outside Global address. Outside local addresses are non-routable private IP addresses.***

**Outside Global Address**- is the address of the outside server as seen on the outside global internet. So for our example the outside local and global would probably look the same. They are the Public IP address of the server on the other side of the internet. 

# Private Ip addresses:
```
Class A: 10.0.0.0 - 10.255.255.255
Class B: 172.16.0.0 - 172.31.255.255
Class C: 192.168.0.0 - 192.168.255.255
```
# Public Ip Addresses:
```
Class A: 1.0.0.0 - 9.255.255.255 and 11.0.0.0 - 126.255.255.255
Class B: 128.0.0.0-172.15.255.255 and 172.32.0.0 - 191.255.255.255
Class C: 192.0.0.0 - 192.167.255.255 and 192.169.0.0 - 223.255.255.255
```
Public IP addresses are managed by IANA or Internet Assigned Numbers Authority

# 3 Types of Address Translation:

Static Nat - Translates Private IP addresses to Public IP addresses (one to one address or a specific address)
      example: Used for when a Server inside an internal network needs to be accessible from the internet. You would set a static NAT so any traffic going out of the network would have a specific Global outside address.

Dynamic NAT - Translates Private IP addresses from a pool of Public IP addresses.
      example: When 2 companies merge, they have overlapping private addressing. Instead of restructuring the network they use Dynamic NAT so the devices can talk to eachother. Not efficient and should only be used when necessary. Also uses port numbers. 
      
PAT or NAT Overloading - Translates multiple Private IP addresses to a single Public IP with the help of Port numbers. 
      example: On most home networks, an ISP only assigns you one public IP address. By adding a port numbers to traffic from your local network you can have mulitple devices using the internet at the same time. Whereas with Static NAT, you would have one address, and if it was being traffic would have to wait to use the address. 


**Process: You set ip nat inside, and ip nat outside on the corresponding interfaces. Then you would set the ip nat inside/outside source and either the address, pool and/or set up overloading.**

# Configuring Static NAT:
```
(config)#int f0/1
(config-if)#ip nat outside  (tells router what interface and which direction to NAT the traffic. In this example you would be Natting for traffic going out of fastethernet 0/1. Like ACL's it's based on the port so traffic comes into the port from an endpoint, and out of a port to an endpoint.)
(config-if)#end
(config)#int f0/0
(config-if)#ip nat inside (tells the router where the interface for the inside of our network is so translations can be reversed.)
(config-if)#end
(config)#ip nat inside source static <source Ip address> <global IP address or interface> (set's nat to translate all packets from the source of the packets on the inside interface. Which in our example would be a computer inside our network. Note that you can set it up to translate only TCP or UDP as well, or subnet translation and IPSec-ESP tunnel but that is not within the scope of CCNA at this time.)
      example: ip nat inside source static 10.1.1.1 8.1.1.5 (Nat's our private inside 10 address to a static global public 8 address)
```

# Configuring Dynamic NAT:
```
(config)#int f0/1
(config-if)#ip nat outside
(config-if)#int f0/0
(config-if)#ip nat inside  (setup inside and outside interfaces same as before.)

(config)#access-list {name|number}  permit <ip addresss> <wildcard mask> (set up an access list for what computers are allowed to be natted out of the network and which aren't.)
      example access-list 1 permit 10.1.1.0 0.0.0.255
      
(config)#ip nat pool <name> <ip range> <netmask>
      example: ip nat pool NAT-Pool 8.1.1.5 8.1.1.10 255.255.255.0
      
(config)#ip nat inside source list <access list> pool <poolname>  (applies the pool only to sources inside, and that coincide with access list 1 created earlier in our steps.)
      example: ip nat inside source list 1 pool NAT-Pool
```

# Configuring PAT:
```
(config)#config t
(config-if)#int f0/1
(config-if)#ip nat outside
(config-if)#int f0/0
(config-if)#ip nat inside
(config)#access-list {name|number} permit <ip address> <wildcard mask>
(conifg)#ip nat inside source list interface fastethernet 0/1 overload 
```
Extended NAT is one IP address and port number Natted to another IP address and port number. 

# Troubleshooting/Show Commands:

``show ip nat translations``</br>
``clear ip nat translations * ``(clears all ip nat translations)</br>
``show ip nat statistics``</br>
``show run | include nat``</br>
``show ip nat statistics``
