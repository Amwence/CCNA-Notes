# LLDP:

Link Layer Discovery Protocol (LLDP) is a neighbor discovery protocol that is used for network devices to advertise information about themselves to other devices on the network. This protocol runs over the data-link layer (OSI 2), which allows two systems running different network layer protocols to learn about each other. LLDP supports a set of attributes that it uses to discover neighbor devices. *These attributes contain type, length, and value descriptions and are referred to as TLVs*. LLDP supported devices can use TLVs to receive 
and send information to their neighbors. Details such as configuration information, device capabilities, 
and device identity can be advertised using this protocol.

## The switch supports these basic management TLVs. These are mandatory LLDP TLVs.
• Port description TLV </br>
• System name TLV </br>
• System description </br>
• System capabilities TLV</br>
• Management address TLV</br>
• Port VLAN ID TLV ((IEEE 802.1 organizationally specific TLVs) </br>
• MAC/PHY configuration/status TLV(IEEE 802.3 organizationally specific TLVs)</br>

**Type Length Values (TLVs)**- are blocks of information embedded in CDP advertisements which gives details like address, device-id,platform...

# LLDP configuration: 

```
(config)#lldp holdtime <seconds> (Specify amount the amount of time a receiving device should hod the information sent by your device before discarding)
(config)#lldp reinit (Specify the delay time in seconds for LLDP to initialize on any interface. Range is 2--5 seconds, and default is 2sec)
(config)#lldp timer <seconds> (set the transmission frequency of LLDP updates in seconds. Range is 5-65534sec and default is 30sec.)
(config)#lldp tlv-select (Specify the LLDP TLVs to send or receive)
(config)#no lldp run (disables lldp globally)
(config)#lldp run (Enables lldp globally)
(config-if)#no lldp transmit (No LLDP packets are sent on interface)
(config-if)#no lldp receive (No LLDP packets are received on interface)
(config-if)#lldp transmit (LLDP packets are sent on the interface)
(config-if)#lldp received (LLDP packets are received on the interface)
```

# Troubleshooting/Show Commands:
``clear lldp counters`` (reset traffic counters to zero)</br>
``clear lldp table`` (Delete LLDP table of information about neighbors)</br>
``show lldp`` (Display global information, such as frequency of transmissions, the holdtime for packets being sent, and the delay time for LLDP to initialize on an interface.)</br>
``show lldp entry <entry-name>`` (Display information about a specific neighbor. You can use * to display all neighbors, or you canter the name of the neighbor)</br>
``show lldp interface [interface-id]`` (Display information about interfaces where LLDP is enabled. or you can choose an interface with the id like fastEthernet 0/0.)</br>
``show lldp neighbors [interface-id] [detail]`` (Display information about neighbors, inluding device type, interface type, and number, holdtime settings, capabilities, and port ID. You can limit the display to specific interface or expand to provider more detail)</br>
``show lldp traffic`` (Display LLDP counters, including the number of packets sent and received, number of packets discarded, and number of unrecognized TLVs.)</br>
