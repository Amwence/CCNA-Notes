# Virtual Router Redundancy Protocol (VRRP)

An open-standard alternate to Cisco's HSRP, Providing same functionality.
(VRRP routers are known as master and backup and only one router can be master at a time. Can support up to 255 virtual router instances
on a single device)

# VRRP Configuration:
```
interface FastEthernet0/0
ip address 10.0.1.2 255.255.255.0
vrrp 1 ip 10.0.1.1 (Virtual IP for VRRP)
vrrp 1 timers {advertise <hello> | learn}
vrrp 1 priority <priority>   (higher priority is better.range is 1-254 Master priority>Backup priority if a tie higher IP address wins)
vrrp 1 preempt (enable by default)
vrrp 1 authentication md5 key-string <password>
vrrp 1 track <object> decrement <value>
```
```
Vrrp IPv6 support: No
Load Balancing: No
Transport: IP/ port 112
Default priority: 100
Default hello: 1sec
multicast group: 224.0.0.18
```

# Troubleshooting/show commands:

``show vrrp [brief]``</br> 
``show track [brief]``

## Other notes:

VRRP Object Tracking
Like HSRP, VRRP supports the ability to alter device priority, depending on the state of a currently configured tracking object. 
At its most basic, this object can track the line protocol state or IP configuration state of an interface and go up or down depending
on these states (specific states can be tracked using Cisco's IP SLA feature). Once configured, the VRRP group will continue to pool 
the track object for its status. If it is down, it can be configured to alter the priority of a specific VRRP device, which can affect 
the current device that is being selected as the master router. 
