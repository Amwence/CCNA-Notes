# Virtual Local Area Network (VLAN)

A virtual LAN is any broadcast domain that is partitioned and isolated in a computer network at the data link layer (OSI layer 2). VLANs work by applying tags to network frames and handling these tags in networking systems-creating the apearance and functionality of network traffic that is physically on a single network, but acts as if it is split between seperate networks. In this way, VLANs can keep network applications separate despite being connected to the same physical network, and without requiring multiple sets of cabling and network devices deployed. 

***Note**:</br> 
*tagged/untagged traffic. This is terminology used by everyone else but cisco and I found it a bit confusing. So frames only get tagged when they go over a trunk port, because logically the switch treats each vlan as a seperate switch, so you wouldn't need to tag a port within it's own switch; or atlest that is the way that I think of it. So think of each VLAN as it's own virtual device, and it will not talk with any other VLANS without either going over a trunk port to another switch, or if you set up intervlan routing (OSI layer 3). The trunk will see that a frame came from a tagged port and tag it with the corresponding VLAN before sending it over the trunk. Any traffic untagged that gets sent over is tagged with the Native VLAN of the switch that recieves it. So if one switch has a native vlan of 20 and sends a frame over to a switch with a VLAN of 30 the frame will be tagged at the destination switch with the 30 vlan tag. This is because a trunk port does not tag native VLANs as it sends them out. Similairly if you you set a native vlan of 10 out of a switchport and you did a corresponding packet capture on that port you will see that the packet will not have a vlan tag, and the vlan tag will be added at the destination switch. This is why you need your Native Vlans to match on both sides of a trunk link. Another thing to note is that endpoints do not know what VLAN they are on and it is all handled at the switchport(with the exception of some IP phones which have a switch within them).*

# Terminology:

**Trunking**: Carrying multiple VLANs over the same physical connection

**Native VLAN**: By default, frames in this VLAN are untagged when sent across a trunk link

**Access VLAN**: The VLAN to which an access port is assigned

**Voice VLAN**: If configured, enables minimal trunking to support voice traffic in addition to datta traffic on an access port

**Dynamic Trunking Protocol**: Can be used to automatically establish trunks between capable ports

**Switched Virtual Interfaces (SVI)**: A virtual interface which provides a routed gatewy into and out of a VLAN

# Switchport Modes:
**trunk**: Forms and unconditional trunk

**dynamic desirable**: Attempts to negotiate a trunk with the far end

**dynamic auto**: Forms a trunk only if requested by far end

**access**: will never form a trunk

# VLAN Creation:
```
Switch(config)# vlan <1-4096>
switch(config-vlan)#name <name>
```
# Access Port Configuration:
```
interface FastEthernet 0/0
switchport mode access
switchport mode nonegtiate (stops trunk negotiation)
switchport access vlan <1-4096> ( can do more than one vlan: vlan 1, 2, 10)
switchport voice vlan <1-150> 
```
# Trunk Port Configuration:
```
interface fastEthernet 0/0
switchport mode trunk
switchport trunk encapsulation dot1q
switchport trunk allowed vlan 10,20-30
switchport trunk native vlan 10
```
# SVI configuration:
```
switch(config)#interrface vlan <1-4096>

switch(config-if)#ip address 192.168.100.1 255.255.255.0 ( Ip address of virtual interface and corresponding subnet mask)
```
# VLAN trunking Protocol (VTP)

**Domain**: common to all switches participating in VTP

**Server Mode**: Generates and propogates VTP adertisements to clients; default mode on unconfigured switches

**Client Mode**: Recieves and forwards andvertisements from servers; VLANs cannot be manually configured on switches in client mode

**Transparent Mode**: Forwards advertisements but does not participate in VTP; VLANS must be configured manually

**Pruning**: VLANs not having any access ports on an end switch are removed from the turnk to reducce flooded traffic

# VTP configuration:

```
switch(config)#vtp mode {server | client | transparent}

switch(config)#vtp mode <domain> (must match for vtp to work)

switch(config)#vtp password <password>

switch(config)#vtp version {1|2} (Version 1 will not forward in transparent mode unless domains match and 2 will forward them regardless. The main difference between VTP Version 3 and VTP Version 2 is that the version 3 supports extended range VLANs. Thus, supporting up to 4k VLANs. It also supports for the creation and advertisement of private VLANs)

switch(config)#vtp pruning
```
**Vlan1** is default vlan

**Vlan2-1001** is the normal vlan range. You can create, edit, and delete it.

**vlan1002-1005** are the default ranges for cisco Token rings and FDDI. You cannot delete this vlan

**vlan 1006-4094** is the extended range of vlans.
