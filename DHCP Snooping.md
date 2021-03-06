# DHCP Snooping
DHCP snooping is a security feature that acts like a firewall between untrusted hosts and trusted DHCP 
servers. The DHCP snooping feature performs the following activities: 

• Validates DHCP messages received from untrusted sources and filters out invalid messages. 

• Rate-limits DHCP traffic from trusted and untrusted sources.

• Builds and maintains the DHCP snooping binding database, which contains information about 
untrusted hosts with leased IP addresses. 

• Utilizes the DHCP snooping binding database to validate subsequent requests from untrusted hosts.
Other security features, such as dynamic ARP inspection (DAI), also use information stored in the 
DHCP snooping binding database.

**DHCP snooping is enabled on a per-VLAN basis**. By default, the feature is inactive on all VLANs. You 
can enable the feature on a single VLAN or a range of VLANs.

The DHCP snooping feature is implemented in software on the MSFC. Therefore, all DHCP messages 
for enabled VLANs are intercepted in the PFC and directed to the MSFC for processing.


#Configure DHCP snooping:
```
(config)#ip dhcp snooping
(config)#ip dhcp snooping {database|information|verify|vlan} <number if vlan>
(config)#interface <interface>
(config-interface)#ip dhcp snooping {limit|trust}
(config)#no ip dhcp snooping information option
```
Examples:
```
(config)# ip dhcp snooping (sets snooping globally across the device. Interfaces will not be able to get DHCP offers without being set up first. So via vlan or what have you.)

(config)# ip dhcp snooping vlan 10 (sets dhcp snooping for vlan 10. Allows you to set trusted ports in a different command. Otherwise the trusted port would be available globally)


(config) #int gig 0/0/0 (choose a trusted interface for dhcp)
(config-if)# ip dhcp snooping trust (makes the gig 0/0/0 a trusted port for dhcp on vlan 10 no other servers can give vlan 10 dhcp offers without being trusted.)
```
</br>

# Troubleshooting/Show Commands:
``show ip dhcp binding``</br>
``show ip dhcp database {detail}``

## Resources:

https://www.cisco.com/c/en/us/td/docs/switches/lan/catalyst6500/ios/12-2SXF/native/configuration/guide/swcg/snoodhcp.pdf
