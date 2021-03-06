# Loopback
What is a Loopback: A loopback interface is a logical, virtual interface in a Cisco Router. A loopback interface is not a physical interface like Fast Ethernet interface or Gigabit Ethernet interface.

A loopback interface has many uses. Loopback interface’s IP Address determines a router’s OSPF Router ID. A loopback interface is always up and allows Border Gateway Protocol (BGP) neighborship between two routers to stay up even if one of the outbound physical interface connected between the routers is down.

Loopback interfaces are used as the termination points for Remote Source-Route Bridging (RSRB), and Data-Link Switching Plus (DLSW+). Loopback interfaces interfaces are always up and running and always available, even if other physical interfaces in the router are down.

A loop back interface is a software interface which can be used to emulate a physical interface. By default, router doesn’t have any loopback interfaces (loopback interfaces are not enabled by default), but they can easily be created.

Loopback interfaces are treated similar to physical interfaces in a router and we can assign IP addresses to them. The command syntax to create a loopback interface is shown below.

(Used for testing TCP stack is a virtual interface and will never fail. Used for determining OSPF Router ID, and is a great back up even if you manually set the router ID. Incase a mistake is made and the ID gets removed the Loopback will be next to assign the ID. Also allows neighbor relationships to stay up even if links go down if redundancy is implemented, because a virtual interface will never go down unless taken down administratively.)

# Loopback configuration:
```
(config)#int loopback <number>
(config-int)#ip address <ip address> <subnet mask>
```