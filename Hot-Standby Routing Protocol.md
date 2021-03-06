# Hot Standby Router Protocol (HSRP)
Provides Default gateway redundancy using one active and one standby router; standardized but liscensed by Cisco. 
(only one router can be active in a group, but can have up to 16 individual routers within that group)


# Hot-Standby Routing protocol configuration:
```
interface FastEthernet0/0
ip address 10.0.1.2 255.255.255.0 (arbitrary ip address and subnet mask)
standby version {1|2}
Standby 1 ip 10.0.1.1 (virtual Ip address for Router)
standby 1 timers <hello> <dead>
standby 1 priority <priority> (default is 100 and a bigger number is what is chosen. Active priority>Standby priority)
standby 1 preempt (if router goes down and back up makes it choose active roll when coming back up not configured by default)
standby 1 authentication md5 key-string <password>
standby 1 track <object> decrement <value>
```

###Supports IPv6: Yes
###Transport: UDP/port: 1985
###Default priority: 100 (bigger number is better)
###Default Hello timer: 3 sec
###multicast group: 224.0.0.2

# Troubleshooting/Show Commands:
```show standby [brief]``</br>  
``show track [brief]``
