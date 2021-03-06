 # Port Security

 You can use the port security feature to restrict input to an interface by limiting and identifying MAC addresses of the workstations that are allowed to access the port. When you assign secure MAC addresses to a secure port, the port does not forward packets with source addresses outside the group of defined addresses. If you limit the number of secure MAC addresses to one and assign a single secure MAC address, the workstation attached to that port is assured the full bandwidth of the port.

If a port is configured as a secure port and the maximum number of secure MAC addresses is reached, when the MAC address of a workstation attempting to access the port is different from any of the identified secure MAC addresses, a security violation occurs.

After you have set the maximum number of secure MAC addresses on a port, the secure addresses are included in an address table in one of these ways:

•You can configure all secure MAC addresses by using the switchport port-security mac-address mac_address interface configuration command.

•You can allow the port to dynamically configure secure MAC addresses with the MAC addresses of connected devices.

•You can configure a number of addresses and allow the rest to be dynamically configured. 

 Port Security Guidelines and Restrictions

Follow these guidelines when configuring port security:

•A secure port cannot be a trunk port unless nonegotiate and 802.1Q.

•A secure port cannot be a destination port for Switch Port Analyzer (SPAN).

•A secure port cannot belong to an EtherChannel port-channel interface.

•A secure port and static MAC address configuration are mutually exclusive. 



##  When configuring port security violation modes, note the following information:


•protect—Drops packets with unknown source addresses until you remove a sufficient number of secure MAC addresses to drop below the maximum value.

•restrict—Drops packets with unknown source addresses until you remove a sufficient number of secure MAC addresses to drop below the maximum value and causes the SecurityViolation counter to increment.

•shutdown—Puts the interface into the error-disabled state immediately and sends an SNMP trap notification. 

# How to configure port-security: 

```1
(config)#int <interface>

(config)#errdisable recovery cause psecure-violation 
(sets recovery option for a port affected by port security shutdown. So that it will come up without having to shut no shut the interface.)

(config)#errdisable recovery interval <sec> 
(sets how long to disable a port that is affected by port security shutdown.)

(config-if)#mac-address <mac-address>
(sets a specific mac address for a port. Not necessary for configuring port security but good to know)


(config-int)#switchport port-security 
(Must use this command before using any of the others to turn it on on the port. By default max of 1 address allowed on port, and shutdown-error disables the port if another device is plugged in with different mac.Does not work on a dynamic port. So you need to set the port as either access or trunk cannot be left up to dtp)

(config-int)#switchport port-security aging time <1-1440 minutes> 
(sets how long to remember a mac-address for the port-security)

(config-int)#switchport port-security maximum <1-132 value> 
(sets maximum value of secure mac addresses for the port. Default is 1.)

(config-int)#switchport port-security violation {portect|restrict|shutdown} 
(Sets what happens if there is a violation. Restrict-A port security violation restricts data and causes the SecurityViolation counter to increment and send an SNMP trap notification. Shutdown- The interface is error-disabled state when a security violation occurs. Must shut no shut the port to get it to come back up. )

(config-int)#switchport port-security mac-address <mac-address> 
(sets a specific mac address for the port security)
```

# Troubleshooting/Show Commands:
``show port-security``</br>
``show port-security address``</br>
``show mac address-table``</br>
``show interface status``</br>
``clear port security {dynamic|all}``</br>
