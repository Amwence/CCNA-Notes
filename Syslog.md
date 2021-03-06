# Syslog:

Is a standard for message logging. It allows separation of the software that generates messages, the system that stores them, and the software that reports and analyzes them. Each message is labeled with a facility code, indicating the type of system generating the message, and is assigned a severity level.

Computer system designers may use syslog for system management and security auditing as well as general informational, analysis, and debugging messages. A wide variety of devices, such as printers, routers, and message receivers across many platforms use the syslog standard. This permits the consolidation of logging data from different types of systems in a central repository. Implementations of syslog exist for many operating systems.

When operating over a network, syslog uses a client-server architecture where a syslog server listens for and logs messages coming from clients.

# Syslog Configuration:
```
(console)#logging host <ip address> (Set syslog server ip address and paramaters)
(console)#logging on (Enable logging to all enabled destinations)
(console)#logging userinfo (enable logging of user info on privledged mode enabling)
(console)#service timestamps log datetime msec (Enables Timestamps to be sent with Syslogs)
(console)#logging trap <level> (limits messages logged to the terminal lines. By default, the terminal receives debugging messages and numerically lower levels.)
(config-line)logging synchronous (done on line con 0 and makes it so messages don't interrupt what you are typing. Brings whatever you were typing back up under whatever message shows up.)
(config)#no logging console
```

# Syslog:

## 0 Emergencies - System is unusable
## 1 Alerts - Action must be taken immediately
## 2 Critical - Critical Conditions
## 3 Error - Error conditions
## 4 Warnings - Warning conditions
## 5 Notifications - Normal but significant condition
## 6 Informational - Informational messages
## 7 Debugging - Debug-Level messages


**Every Awesome Cisco Engineer Will Need Icecream Daily**
