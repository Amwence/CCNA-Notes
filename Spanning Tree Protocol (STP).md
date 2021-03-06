# Spanning Tree Protocol

The spanning tree protocol is a network protocol that builds a loop-free logical topology for Ethernet Networks. The basic function of STP is to prevent bridge loops(switch) and the broadcast radiation that results from them. Spanning tree also allows
a network design to include backup links providing fault tolerance if an active link fails.

# Types of STP:
```
            Legacy STP      PVST      PVST+     RSTP      RPVST+      MST
Algorithm   Legacy ST       LegacyST  Rapid ST  Rapid ST  Rapid ST    Rapid ST

Defined by  802.1D-1998     Cisco     Cisco     802.1w,   Cisco       802.1s,
                                                802.1D-2004           802.1Q-2003

Instances       1           per Vlan  PerVlan        1    PerVlan     

Configurable

Trunking        N/A         ISL       802.1Q,ISL  N/A     802.1Q,ISL  802.1Q,ISL
```

# Spanning Tree Operation:
1. Determine the Root Bridge- the bridge advertising the lowest bridge ID becomes the root bridge

2. Select Root Port- each bridge selects its primary port facing the root

3. Select Designated Ports- one designated port is selected per segment

4. Block Ports with Loops- all non-root and non-designated ports are blocked

# Port States:

**Legacy ST**- Disabled, Blocking, Listening, Learning, Forwarding

**Rapid ST**- Discarding, Learning, Forwarding

# Port Roles:

**Legacy ST**- Root, Designated, Blocking

**Rapid ST**- Root, Designated, Alternate, Backup

## Default Timers:
**Hello**: 2sec
**Forward Delay**: 15sec
**Max Age**: 20sec

# Link Cost:
<table>
<tr>
<th>Bandwidth:</th>                       <th>Cost:</th>
</tr>
<tr>
<td>4Mbps</td>                            <td>250</td>
</tr>
<tr>
<td>10Mbps</td>                           <td>100</td>
</tr>
<tr>
<td>16Mbps</td>                           <td>62</td>
</tr>
<tr>
<td>45Mbps</td>                           <td>39</td>
</tr>
<tr>
<td>100Mbps</td>                          <td>19</td>
</tr>
<tr>
<td>155Mbps</td>                          <td>14</td>
</tr>
<tr>
<td>622Mbps</td>                          <td>6</td>
</tr>
<tr>
<td>1Gbps</td>                            <td>4</td>
</tr>
<tr>
<td>10Gbps</td>                           <td>2</td>
</tr>
<tr>
<td>20+ Gbps<td>                          <td'>1</td>
</tr>
</table>

# Path Selection:

## 1. Bridge with lowest root ID becomes the root

## 2. Prefer the neighbor with the lowest cost to root

## 3. Prefer the neighbor with the loweset bridge ID

## 4. Prefer the lowest sender port ID

## **BPDU's** use a uinque mac address of 1:80:c2:00:00:00 or for **PVST** 01:00:0C:cc:CC:cd

# PVST+ and RPVST+ Configuration:

```
!set mode
spanning-tree mode {pvst | rapid-pvst}

!Bridge Priority
spanning-tree vlan <1-4094> priority <0-61440> (must be in increments of 4096 and default is 32768)

!Timers, in seconds. I listed default 
spanning-tree vlan <1-4094> hello-time 2<seconds>
spanning-tree vlan <1-4094> forward-time 15<seconds>
spanning-tree vlan <1-4096> max-age 20<seconds>

!PVST+ Enhancements
spanning-tree backbonefast (Enables immediate expiration of max age timer in the event of an indirect link failure)
spanning-tree uplinkfast (Enables switches to maintain backup paths to root)

! Interface Enhancements, Defaults listed
spanning-tree [vlan 1-4096] port-priority 128 <0-240>
spanning-tree [vlan 1-4096] cost 19 <1-200,000,000> (default cost depends on the type of link in this example its a 100Mbps switchport. See table above)

!Manual Link Type Specification                     
spanning-tree link-type {point-to-point | shared} (Point-to-Point-Connects to exactly one other bridge[Full-duplex]. Shared potentially connects to multiple bridges[Half-duplex]. Edge connects to a single host; designated by Portfast)

!Enables Portfast if running PVST+, or designates and edgeport under RPVST+
spanning-tree portfast (skips listening/discarding states and goes straight to forwarding.)

!Spanning tree protection
spanning-tree gaurd {loop | root | none}

!Per-interface toggling
spanning-tree bpduguard enable
spanning-tree bpdufilter enable
```
# Spanning Tree Protection:

**Root Guard**: Prevents a port from becoming the root port

**BPDU Guard**: Error-Disables a port if a BPDU is received

**Loop Guard**: Prevents a blocked port from transitioning to listening after the Max Age timer has expiredf

**BPDU Filter**: Blocks BPDu's on an interface (disables STP)

# MST Configuration:
```
spanning-tree mode mst

!MST configurationg
spanning-tree most configuration
name <MyTree>
revision 1

!Map Vlans to instances
instance 1 vlan 20, 30
instance 2 vlan 40, 50

!Bridge priority (per instance)
spanning-tree mst 1 priority 32768 <same priority limitations as pvst>

!Timers, in seconds
spanning-tree mst hello-time 2
spanning-tree mst forward-time 15
spanning-tree mst max-age 20

!Maximum hops for BPDUs
spanning-tree mst max-hops 20

!interface attributes
interface FastEthernet 0/1
spanning-tree mst 1 port-priority 128
spanning-tree mst 1 cost 19
```
# Troubleshooting/Show commands:
``show spanning-tree [summary | detail | root]``</br>
``show spanning-tree [interface | vlan]``</br>
``show spanning-tree mst [...]``</br>
