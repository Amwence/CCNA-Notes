# Port mirroring (SPAN):

Is used on a network switch to send a copy of network packets seen on one switch port (or an entire VLAN) to a network monitoring connection on another switch port. This is commonly used for network appliances that require monitoring of network traffic such as an intrusion detection system, passive probe or real user monitoring (RUM) technology that is used to support application performance management (APM). Port mirroring on a Cisco Systems switch is generally referred to as Switched Port Analyzer (SPAN) or Remote Switched Port Analyzer (RSPAN). Other vendors have different names for it, such as Roving Analysis Port (RAP) on 3Com switches.

Network engineers or administrators use port mirroring to analyze and debug data or diagnose errors on a network. It helps administrators keep a close eye on network performance and alerts them when problems occur. It can be used to mirror either inbound or outbound traffic (or both) on single or multiple interfaces.

# SPAN Configuration: 

## On a switch:
```
(console)#monitor session <number> {source | destination} {interface | remote | vlan <1-4096>} { , (sequence separator | - (range seperator) | both | rx (SPAN copies only ingress traffic) |tx (SPAN copies only egress traffic)}

example: monitor session 1 source vlan 1 both (sets monitor session to 1 and monitors both ingress and egress traffic on this switches vlan 1 since it is the source)

example: monitor session 1 destination interface fastEthernet 1/0/5 (sends the monitored session from previous example to this interface)

(console)#monitor session <number> source interface {,|-|encapsulation|ingress}

(console)#monitor session <number> destination interface {, | - | encapsulation (set encapsulation for destination traffic forwarding)| ingress (enable ingress traffic forwarding}
```


***Note:***</br>

***Don't want to Span a Gig port to one slower because you could overwhelm the port with traffic.***</br>

**Egress**- *traffic leaving*</br>

**ingress**-*traffic coming in*</br>

*A span destination port can only be used with one span session at a time*</br>

*A span destination port can also not be a span source port*</br>

*Span Destination port- The switch no longer treats that port like a normal ethernet port. Mac addresses are no longer learned on that port. Traffic received on that port that port is not accepted by default.*</br>

*To remove a span destination port you have to no monitor session <number> <destination interface>*</br>

*You can move a span destination port from 1 session to another*</br>

*Can use span with multiple sources-can be used with a single span session*</br>

*Cannot mix interfaces and vlan sources- need to have seperate sessions if you want multiple interfaces and vlans.*

*Can have only multiples of one or the other in a span session, cannot mix multiple vlans with multiple ports.*

*Etherchannel and trunks can be used for SPAN.*







