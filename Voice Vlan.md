# Voice Vlan:
```
(config)#int range <interface range>
(config-if)#switchport trunk encapsulation dot1q
(config-if)#switchport mode trunk
(config-if)#switchport trunk native vlan <1-4094>
(config-if)#switchport voice vlan <1-4096>
(config-if)#switchport trunk allowed vlan <native Vlan>,<voice Vlan>, <othervlan if needed>
(config-if)#mls qos trust {cos|dscp|device}
(config)# do show run
(config)# do show interface <interface> switchport

(config)#int range <interface range>
(config-if)#switchport mode access
(config-if)#switchport voice vlan <1-4096>
(config-if)#switchport access vlan <1-4096>
(config-if)#mls qos trust {cos|dscp|device}
```