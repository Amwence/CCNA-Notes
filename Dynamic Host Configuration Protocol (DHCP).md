# Dynamic Host Configuration Protocol:
is a network management protocol used on Internet Protocol (IP) networks for automatically assigning IP addresses and other communication parameters to devices connected to the network using a client–server architecture.
The technology eliminates the need for individually configuring network devices manually, and consists of two network components, a centrally installed network DHCP server and client instances of the protocol stack on each computer or device. When connected to the network, and periodically thereafter, a client requests a set of parameters from the DHCP server using the DHCP protocol.

## DHCP uses DORA: 
**Discovery**- Client sends out a DHCPDiscover message on a netwrok using the destination 255.255.255.255 broadcast.

**Offer**- When a DHCP server recieves a discover message; the server makes lease offer by sending the DHCPOFFER message which contains Client id (traditionally mac address), The Ip address the server is offering, the subnet mask, the lease duration, and the ip address of the DHCP server making the offer. 

**Request**- In response to the DHCP offer, the client replies with a DHCPREQUEST messsage, broadcast to the server. A client can receive offers from multiple servers, but it will accept only one DHCP offer. The client will produce a gratuitous arp in order to find if there is any other host present in the network with the same IP address. If there is no reply by other hosts, then there is no host with the same ip configuration and the message is broadcast to server showing the acceptance of the ip address. 

**Acknowledge**- When the DHCP server recieves the DHCPREQUEST the configuration enters it's final stage. The server sends the DHCPACK packet back to the client which includes the lease duration, and any other configuration information the client might have requested. At this point the IP configuration process is completed. 


----------------------------------------------------------------------------------------------------------------------------------------------

## Things to Note:

DHCP server can have more than one client pool.

A client usually asks for a new lease on their ip when they are halfway through the lease time. 

The DHCP server identifies which DHCP address pool to use to service a client request as follows:

If the client is not directly connected (the giaddr field of the DHCPDISCOVER broadcast message is non-zero), the DHCP server matches the DHCPDISCOVER with a DHCP pool that has the subnet that contains the IP address in the giaddr field.(Subnet of whatever device relayed messaged. Hence why you may want to set a helper address on a vlan interface and set up intervlan routing.)

If the client is directly connected (the giaddr field is zero), the DHCP server matches the DHCPDISCOVER with DHCP pool(s) that contain the subnet(s) configured on the receiving interface. If the interface has secondary IP addresses, the subnets associated with the secondary IP addresses are examined for possible allocation only after the subnet associated with the primary IP address (on the interface) is exhausted.(Uses subnet of directly connected device, and if it has more than one address uses the primary one until all address are exhausted from that pool.)


-----------------------------------------------------------------------------------------------------------------------------------------------


# DHCP Configuration:
```
(console)#ip dhcp excluded-address <ip address> <ip address> (Configures excluded addresses from a range of Ip addresses)
(console)#ip dhcp pool <name> (creates DHCP pool)
(dhcp-config)#network <ip address> <subnet mask> (Creates the Subnet DHCP uses to give clients IP addresses and takes into consideration the Excluded addresses)
(dhcp-config)#domain-name <domain> (Sets domain name to give to client)
(dhcp-config)#dns-server <address> [address 2...address8] (Sets dns server to give to client)
(dhcp-config)#default-router <address> (sets default router for client)
(dhcp-config)#option <Raw DHCP options 0-254> (Other DHCP options not covered/more research required)
```
# Configuring DHCP Helper-Address/Relay Agent: 

```
switch(config-if)#ip helper-address <address>
```

*Note: DHCP IP Helper addresses are IP addresses configured on a routed interface such as a VLAN Interface or a routers Ethernet interface that allows that specific device to act as a “middle man” which forwards BOOTP (Broadcast) DHCP request it receives on an interface to the DHCP server specified by the IP Helper address. (used by level 2 switches and routers that aren't set for dhcp)*

## Router:
Routers drop broadcast packets by default in order to send them on to the DHCP server/router you would need to set a helper address to configure that router as a relay agent. Same for layer 3 switch below on the vlan interface.
```
router(config)#interface <interface>
(config-if)#ip helper-address <address>
```
## Layer 3 switch:

Used for specific vlans
```
switch(config)#interface vlan <1-4096>
switch(config-if)#ip helper-address <address>
```

# Troubleshooting/ Show Commands:
``show ip dhcp binding`` (displays all bindings created)</br>
``show ip dhcp binding <address>`` (displays the binding for a specific dhcp client with the selected ip address)</br>
``clear ip dhcp binding <address>`` (clears automatic address binding from the dchp server database)</br>
``clear ip dhcp binding *`` (clears all automatic DHCP bindings)</br>
``show ip dhcp conflict`` ( displays a list of all address conflicts recorded by the server)</br>
``clear ip dhcp conflict <address>`` (clears address conflict from the database)</br>
``clear ip dhcp conflict *`` (clears all dhcp conflicts from database)</br>
``show ip dhcp database`` (displays recent activity on the dhcp database)</br>
``show ip dhcp server statistics`` (displays a list of the number of messages sent and received by the dhcp server)</br>
``clear ip dhcp server statistics`` (resets server statistics)</br>
``debug ip dhcp server {events | packets| linkage | class}`` (displays the dhcp process of addresses being leasesd and returned)</br>

***NOTE:***
*Some of the Show/Clear commands above may not work on the router and would only work on a server.There are many other DHCP commands when setting up a server that I did not include.*
