# Open Shortest Path First (OSPF):
OSPF is a routing protocol for Internet Protocol (IP) networks. It uses a link state routing (LSR) algorithm and falls into the group of interior gateway protocols (IGPs), operating within a single autonomous system (AS). It is defined as OSPF Version 2 in RFC 2328 (1998) for IPv4. The updates for IPv6 are specified as OSPF Version 3 in RFC 5340 (2008). OSPF supports the Classless Inter-Domain Routing (CIDR) addressing model.

OSPF is a widely used IGP(Interior Gateway Protocol) in large enterprise networks. IS-IS, another LSR(Link State Routing)-based protocol, is more common in large service provider networks.

OSPF is a link state routing protocol that will flood the area with Link State Advertisements (LSAs). Link being the router interface and the state being a description of an interface and it's relationship to neighboring routers. Is the link up/down, Ip address, subnetmask, routers connected to it, etc.

Uses Dijikstras algorithm or known as Shortest Path First(SPF)

Creates neighbor relationships by sending out multicast address 224.0.0.5-6 or unicast.

# OSPF Tables

**IP OSPF Neighbor Table (Adjacency Table)**- Neighboring Routers

**IP OSPF Topology Table (LSDB Link State Database)**- All routers and attached links in an area/network

**IP OSPF Routing Table (Fowarding Table)**- Best Routes. (All ospf routers in the same area share the same database)

# OSPF Packet Types:

**Hello**- Dynamically discovers neighbors and forms/maintains neighbor relationships. Default interval for broadcast over ethernet segments is 10 seconds and 30 seconds for non-broadcast segments like serial, NBMA (non broadcast multi access). ***Dead timer is 4 times the hello interval by default***. If you change the hello interval OSPF will automatcially change the dead interval. By default it is 40 seconds over ethernet and 120seconds over non-broadcast seconds. If a hello is used and does not get a response within the dead interval the neighbor relationship is torn down and the databases are updated. 

**Database Description Packet(DBD/DD)**- Used to exchange brief versions of each LSA or the state of links. 

**Link State Request (LSR)**- If information is missing from a router's Link State Database, it will then send a LSR for the Full Link State Advertisment to make updates to its LSDB or linkstate database. The neighboring router will send a LSU or Link State Update. 

**Link State Update**- Contains Link State Advertisements, and typically is in response to a LSR or Link State Requeset.

**Link State Acknowldedgment (LSAck)**- Confirmation of reciept of an LSU message (LINK STATE UPDATE)


Cisco Recommends no more than 50 routers within a single OSPF area. OSPF uses a Heirarchical model meaning that you will always have an Area 0 if you have more than one OSPF area. All border areas must connect back to the backbone area of Area 0. The flooding of LSAs is broken up by breaking the network up into different areas. Routers that border the backbone area and a different area are known as ABR's or Area Border Routers. ABR's benefit from route summarization, and will summarize some/not all routes not directly connected to them for their routing database.  All traffic destined for a different area than the one it originated in will have to travers area 0 to get to a different area. 

**Autonomous System Border Router**- A router on the border of an Autonomous system and another Autonomous network. An Autonomous system is a different network, not directly managed by the same people, or a router system using a different routing protocol like RIP. For example would be a router on the border of your network and your ISP's, or a system within the network using a different routing protcol like RIP, or EIGRP, etc. 

**Internal Routers**- are routers internal to their specific area. These areas will either need to be completely alone, or connect to the backbone area with an ABR to traverse the network. At Which point the traffic will need to traverse area 0 to get to a different area. For example you have 4 areas. Area 0,1,2,3. area 1 wants to speak with a device in area 3. The device in area 1 would send it's traffic through the area 3 network to the area border router, which will then send it to area 0 and choose the best path to get it to area 3. After it gets to area 3's ABR it will traverse the area 3 network till it gets to where it needs to go. 

In order for neighbor relationships to be formed certain paramaters must match on both routers. The area of the router, Hello and Dead intervals, Authentication password, and Stub area flag. 

SPF or Dijkstra's algorithm makes routing decisions by putting each router at the root of a tree and calculating best possible route depending on link speed, and number of hops. 

# OSPF Neighbor adjancencies hello packet:


**Router ID**- Identifies a specific router, and is used in the election of a designated router and/or backup designated router. Chosen based on the IP address of the highest configured interface, or on the highest loopback interface configured on the router, or can be manually specified using the Router ID command. 

**Hello and Dead intervals**- Default of 10seconds and 40seconds for ethernet interfaces, and 30seconds and 120seconds for non broadcast interfaces. Must match in order to form adjacency.

**Neighbors**- List of neighbors that the router knows about. 

**Area ID**- Must match on both routers to form an adjacency. Higher number is better and is chosen by either manually setting it, The highest configured loopback address at the time ospf is started, or the highest Ip address of an interface when ospf is first started. 

**Router Priority**- Is used for Designated Router and Backup Designated router elections. 

**Designated Router(DR) IP Address**- Designated Router is used in Broadcast multi Access enviornments. It is used to cut down on Broadcasts. All routers send their LSU's to the designated router, and the DR decides to broadcast them out as an update to all routers within an area. If not set up routers would update every router in its LSDB about any changes to the topology, and that router would notify all routers within it's LSDB of the change in it's LSDB, and so on down the line. Having a DR means only one router would get notified and update all other routers of the change in the topology. Instead of a chain of updates from all routers to all other routers with redundant traffic. Only the DR listens on 224.0.0.6, and would send out a multicast update via 224.0.0.5. If the DR goes down the BDR becomes the DR and a new BDR election begins. If the original DR comes back in that time after new BDR election takes place it will become a Drother if Preemption has not been set on that device. Preempt is a setting that allows a router to take back it's role as the DR when coming back from a down state and force another DR/BDR election. 
      Criteria for DR elections:
      Highest priority: 1-255, Default value is 1, and 0 disables the ability to become DR or BDR
      Highest router ID: if 2 routers have the same priority it would choose based upon highest router ID

**Backup Designated Router(BDR) IP address**-  is a router that becomes the designated router if the current designated router has a problem or fails. The BDR is the OSPF router with the second-highest priority at the time of the last election. A given router can have some interfaces that are designated (DR) and others that are backup designated (BDR), and others that are non-designated. If no router is a DR or a BDR on a given subnet, the BDR is first elected, and then a second election is held for the DR.
      Criteria for BDR elections:
      Second Highest priority: 1-255, Default value is 1, and 0 disables the ability to become DR or BDR
      Second Highest router ID: if 2 routers have the same priority it would choose based upon highest router ID

**Authentication Password**- Needs to be the same or a connection will not be formed

**Stub Area Flag**- Determines whether the area is stub area or not. 

# Setting Router ID.

Manually set router ID > Highest configured Loopback interface when ospf is enabled>Highest configured IP address of any interface when ospf is enabled
```
(config)#router ospf <process ID>
(config-router)#router-id <router ID a.b.c.d>
(config)#interface <interface>
```

# Troubleshooting/Show Commands
``Show ip protocols``</br>
``Clear ip ospf processes``</br>
``show ip route``</br>
