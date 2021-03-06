# Three Teir Architecture:

## Core Layer:
Core Layer consists of biggest, fastest, and most expensive routers with the highest model numbers and Core Layer is considered as the back bone of networks. Core Layer routers are used to merge geographically separated networks. The Core Layer routers move information on the network as fast as possible. The switches operating at core layer switches packets as fast as possible. (internet/outside networks)

## Distribution layer:
The Distribution Layer is located between the access and core layers. The purpose of this layer is to provide boundary definition by implementing access lists and other filters. Therefore the Distribution Layer defines policy for the network. Distribution Layer include high-end layer 3 switches. Distribution Layer ensures that packets are properly routed between subnets and VLANs in your enterprise.(between floors in a building)

## Access layer:
Access layer includes acces switches which are connected to the end devices (Computers, Printers, Servers etc). Access layer switches ensures that packets are delivered to the end devices.(between end devices on a floor)

# Benefits of Cisco Three-Layer hierarchical model
The main benefits of Cisco Three-Layer hierarchical model is that it helps to design, deploy and maintain a scalable, trustworthy, cost effective hierarchical internetwork.

Better Performance: Cisco Three Layer Network Model allows in creating high performance networks

Better management & troubleshooting: Cisco Three Layer Network Model allows better network management and isolate causes of network trouble.

Better Filter/Policy creation and application: Cisco Three Layer Network Model allows better filter/policy creation application.

Better Scalability: Cisco Three Layer Network Model allows us to efficiently accomodate future growth.

Better Redundancy: Cisco Three Layer Network Model provides better redundancy. Multiple links across multiple devices provides better redundancy. If one switch is down, we have another alternate path to reach the destination.

(Composed from top to bottom of Core Layer, Distribution Layer, Access Layer. The Core layer groups building distribution. The Distribution layer connects to the access layer to cover multiple floors, and the Access layer is the wiring closet with with cables going to workstations.)

![pic1](https://github.com/Amwence/CCNA-Notes/issues/2#issue-1057697248)



# Spine-Leaf-Leaf-spine:
 is a two-layer network topology composed of leaf switches and spine switches.

 Leaf-spine is a two-layer data center network topology that's useful for data centers that experience more east-west network traffic than north-south traffic. The topology is composed of leaf switches (to which servers and storage connect) and spine switches (to which leaf switches connect). Leaf switches mesh into the spine, forming the access layer that delivers network connection points for servers.

Every leaf switch in a leaf-spine architecture connects to every switch in the network fabric. No matter which leaf switch a server is connected to, it has to cross the same number of devices every time it connects to another server. (The only exception is when the other server is on the same leaf.) This minimizes latency and bottlenecks because each payload only has to travel to a spine switch and another leaf switch to reach its endpoint. Spine switches have high port density and form the core of the architecture.

A leaf-spine topology can be layer 2 or layer 3 depending upon whether the links between the leaf and spine layer will be switched or routed. In a layer 2 leaf-spine design, Transparent Interconnection of Lots of Links or Shortest Path Bridging takes the place of spanning-tree. All hosts are linked to the fabric and offer a loop-free route to their Ethernet MAC address through a shortest-path-first computation. In a layer 3 design, each link is routed. This approach is most efficient when virtual local area networks are sequestered to individual leaf switches or when a network overlay, like VXLAN, is working.

(Each Leaf switch is connected to each spine switch, but Leaf switches do not connect to eachother. Reduces latency within a network. It is different than the Heirachical designs creating less hops for data to travel. Instead of going from access switch, to aggregation switch, to core switch in a topology and back down on the other side; it goes from the leaf switch to the spine switch to the other leaf switch because spines are connected to every leaf. It is better for east/west traffic versus the other method being more suited for north/south traffic.)

![Spine-leaf](https://github.com/Amwence/CCNA-Notes/issues/1#issue-1057682090)

# Collapsed Core:

 In a collapsed core architecture, the core and distribution layers are combined simplifying design. A collapsed core block is one where the core layer of the hierarchy is collapsed into the distribution layer. Here, both distribution and core functions are provided within the same switch devices

![Collapsed Core](https://github.com/Amwence/CCNA-Notes/issues/3#issue-1057702295)

# Network Designs:
Fully connected mesh: All devices are connected to eachother
Hub and spoke: All devices are connected to one central device, but not to eachother. 
Partial Mesh: Devices are all connected, but may not all be connected to eachother directly. 

![Designs](https://github.com/Amwence/CCNA-Notes/issues/4#issue-1057705222)
