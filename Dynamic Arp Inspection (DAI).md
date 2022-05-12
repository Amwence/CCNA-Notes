# Dynamic Arp Inspection
Dynamic ARP Inspection (DAI) is a security feature that validates Address Resolution Protocol (ARP) packets in a network. DAI allows a network administrator to intercept, log, and discard ARP packets with invalid MAC address to IP address bindings. This capability protects the network from certain “man-in-the-middle” attacks.

For DAI to work you need DCHP bindings within the database if dhcp snooping is enabled. 

# Configure Dynamic Arp Inspection:
```
(config)#ip arp inspection {filter|log-buffer|validate|vlan} vlan <1-4096>
(config)#int g0/0
(config-interface)#ip arp inspection trust
```
</br>

# Troubleshooting/Show Commands:

``show ip arp inspection``</br>
``show ip dhcp daatabase``</br>
``show ip dhcp binding``</br>
``show ip dhcp statistics``</br>
