# Gateway Load Balancing Protocol (GLBP)

Supports arbitrary load balancing in addition to redundancy across gateways; Cisco Proprietary

## GLBP Roles:

**Active Virtual Gateway (AVG)**</br>
Answers for the virutal router and assigns virtual MAC addresses to the group</br>

**Active Virtual Forwarder (AVF)**</br>
All routers which forward traffic for the group</br>

# GLBP Load Balancing: 

**Round Robin (default)**</br>
The AVG answers host ARP requests for the virtual router with the nest router in the cycle</br>

**Host-Dependent**</br>
Round-robin cycling is used while a consistent AVF is maintained for each host</br>

**Weighted**</br>
Deterimines the proportionate share of hosts handled by each AVF

# GLBP Configuration:
```
interface FastEthernet 0/0
ip address 10.0.1.2 255.255.255.0
glbp 1 ip 10.0.1.1 (sets virtual IP for GLBP)
glbp 1 timers redirect <redirect> <time-out>
glbp 1 priority <priority>
glbp 1 preempt
glbp 1 forwarder preempt
glbp 1 authentication md5 key-string <password>
glbp 1 load-balancing <method> ( Round-Robin[default], Host-Dependent, Weighted)
glbp 1 weighting <weight> lower <lower> upper <upper>
glbp 1 weighting track <object> decrement <value>
```
## GLBP IPv6 support: Yes
## Load Balancing: Yes
## Transport: udp/ port: 3222
## Default priority: 100
## Default Hello: 3sec
## Multicast Group: 224.0.0.102

# Troubleshooting/show commands: 
``show glbp [brief]``</br>
``show track [brief]``</br

