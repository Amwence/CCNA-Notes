# Network Time Protocol (NTP):
 NTP is a networking protocol for clock synchronization between computer systems over packet-switched, variable-latency data networks. In operation since before 1985, NTP is one of the oldest Internet protocols in current use. NTP was designed by David L. Mills of the University of Delaware.
 NTP is intended to synchronize all participating computers to within a few milliseconds of Coordinated Universal Time (UTC). It uses the intersection algorithm, a modified version of Marzullo's algorithm, to select accurate time servers and is designed to mitigate the effects of variable network latency. NTP can usually maintain time to within tens of milliseconds over the public Internet, and can achieve better than one millisecond accuracy in local area networks under ideal conditions. Asymmetric routes and network congestion can cause errors of 100 ms or more.
 
 # How does NTP work:
 
 NTP uses a heirachical, semi-layered sytem of time sources. Each level of this heirarchy is termed a stratum and is assigned a number starting with zero for the reference clock at the top.   A server synchronized to a stratum n server runs at stratum n + 1. The number represents the distance from the reference clock and is used to prevent cyclical dependencies in the hierarchy. Stratum is not always an indication of quality or reliability; it is common to find stratum 3 time sources that are higher quality than other stratum 2 time sources. A brief description of strata 0, 1, 2 and 3 is provided below:

## Stratum 0
These are high-precision timekeeping devices such as atomic clocks, GPS or other radio clocks. They generate a very accurate pulse per second signal that triggers an interrupt and timestamp on a connected computer. Stratum 0 devices are also known as reference clocks. NTP servers cannot advertise themselves as stratum 0. A stratum field set to 0 in NTP packet indicates an unspecified stratum.

## Stratum 1
These are computers whose system time is synchronized to within a few microseconds of their attached stratum 0 devices. Stratum 1 servers may peer with other stratum 1 servers for sanity check and backup. They are also referred to as primary time servers.

## Stratum 2
These are computers that are synchronized over a network to stratum 1 servers. Often a stratum 2 computer queries several stratum 1 servers. Stratum 2 computers may also peer with other stratum 2 computers to provide more stable and robust time for all devices in the peer group.

## Stratum 3
These are computers that are synchronized to stratum 2 servers. They employ the same algorithms for peering and data sampling as stratum 2, and can themselves act as servers for stratum 4 computers, and so on.
<highlight>The upper limit for stratum is 15; stratum 16 is used to indicate that a device is unsynchronized.</highlight> The NTP algorithms on each computer interact to construct a Bellman-Ford shortest-path spanning tree, to minimize the accumulated round-trip delay to the stratum 1 servers for all the clients.

# Default NTP Configurations:
Broadcast Client: disabled
Client Mode: disabled
Broadcast Delay: 3000 mircoseconds
Time Zone: not specified
Offset from UTC: 0 hours
summertime adjustment: disabled
NTP server: not specified
Authentication mode: Disabled

# NTP Configuration:

Synchronizes clocks between network devices and computer systems over a data network. Uses UDP port 132, unicast, broadcast, Multicast.

```
(config)#ntp master <1-15> (sets stratum number)
(config)#clock timezone <name> <offset hours><minutes> (provides a name for the time zone and offset from UTC)
(config)#clock summer-time <name> {date | recurring}
R1#clock set <HH:MM:SS> (current time)
(config)#ntp source <source>
example: ntp source loopback 0 (all parts in this sets the router as an ntp master and that the loopback is what people would contact to get their ntp information)
(config)#ntp server {ip address | Hostname} (sets where to get ntp information from)
example: ntp server 3.3.3.3 (is the loopback of the router in the above example)
```

# NTP Broadcast-Client Configuration:
Broadcast-based NTP associations should be used when time accuracy and reliability requirements are modest and if your network is localized and has more than 20 clients. Broadcast-based NTP associations are also recommended for use on networks that have limited bandwidth, system memory, or CPU resources.

A networking device operating in the broadcast client mode does not engage in any polling. Instead, it listens for NTP broadcast packets that are transmitted by broadcast time servers. Consequently, time accuracy can be marginally reduced because time information flows only one way.

```
(config)#set ntp broadcastclient enable (Enable NTP Broadcast-Client mode)
(config)#set ntp broadcastdelay <microseconds> (Set the estimated NTP broadcast packet delay. Compensates for any server-to-clent packet latency)
```

# Configuring client mode:
configures device to send time of day request to NTP server.
```
(config)#set ntp server <ip address> (Specify the ip address of the NTP server)
(config)#set ntp client enable (Enable NTP client mode)
```

# Configuring Authentication in Client Mode:
When enabled client sends time of day requests only to trusted NTP servers. 
```
(config)#set ntp key <public key> [trusted | untrusted] md5 <secret key> (configure an authentication key pair for NTP and specify trusted or untrusted)

(config)#set ntp server <ip address> [public key] (set the ip address of the NTP server and the public key)
(config)#set ntp client enable (enables NTP client mode)
(config)#set ntp authentication enable (enables NTP authentication)
```

# Setting the Time Zone:
You can set a time zone for the switch to display the time in that zone. You must enable NTP before you set the time zone. UTC is shown by default. 

```
(config)#set timzone <zone hours>[minutes] (Set Time zone, and offset from UTC)
example: set timezone Pacific -8 
```

Timezone set to 'Pacific', offset from UTC is -8 hours
Enabling Daylight Savings Time Adjustment:
Following U.S. standards, you can have the switch advance the clock one hour at 2:00 a.m. on the first Sunday in April and move the clock back one hour at 2:00 a.m. on the last Sunday in October. You can also explicitly specify start and end dates and times and whether the time adjustment recurs every year.
```
(config)#set summertime enable <zone name> (Enable the daylight saving time clock adjustment)
example: set summertime enable PDT
(config)#set summertime recurring (enable daylight saving time clock adjustment to recurr)
```

To enable the daylight savings clock adjustment that recurs every year on different days or with a different offset than the U.S. standards, perform this task in privledged mode:
```
(config)#set summertime recurring <week> <day> <month> <HH:MM> <week> <day> <month> <HH:MM> <offset>

example: set summertime recurring 3 mon feb 3:00 2 saturday aug 15:00 30

Show command would output the following: 

start: Sun Feb 13 2000, 03:00:00 

end: Sat Aug 26 2000, 14:00:00 

Offset: 30 minutes

Recurring: yes, starting at 3:00am Sunday of the third week of February and ending 
14:00pm Saturday of the fourth week of August.
```
# To enable the daylight saving time clock adjustment to a nonrecurring specific date, perform this task in privledged mode: 

```
(console)#set summertime date <month> <date> <year> <HH:MM> <month> <date> <year> <HH:MM> <offset>
example: set summertime date apr 13 2003 4:30 jan 21 2004 5:30 50
show command would output the following: 
Summertime is disabled and set to ''
Start : Thu Apr 13 2000, 04:30:00
End   : Mon Jan 21 2002, 05:30:00
Offset: 1440 minutes (1 day)
Recurring: no
```
# Disabling the Daylight saving time adjustment: 

```
set summertime disable <zone name>
example: set summertime disable PDT
Clear the Time Zone:
Reset zone settings to UTC
(console)#clear timezone (clears offset of timezone)
```
# Clearing NTP servers:

```
Clears NTP server addresses from NTP server table. 
(console)#clear ntp server <ip addresss | all> (Clears either specific addresses or all)
```
# Disabling NTP:
Disable NTP broadcast-client/client mode on the switch.

```
(console)#set ntp broadcastlient disable
(console)#set ntp client disable
```
# Configuring an NTP server and peer:

```
(console)#ntp server {ip address | ipv6 address | DNS name}
(console)#ntp peer {ip address | ipv6 address | DNS name}
```
# Troubleshooting/Show Commands:

``show ntp`` (verify ntp configuration)</br>
``show ntp associations``
``show ntp status``</br>
``show summertime`` (verify's daylight savings config)</br>
``show timezone`` (verify timezone config)</br>
``show ntp peers``
``show ntp peer-status``
``show ntp statistics {io | local | memory | peer | ip address | DNS name}``
``clear ntp statistics``
``clear ntp session``

## Resources/More Info: 

https://www.cisco.com/c/en/us/td/docs/switches/datacenter/nexus1000/sw/4_0/system_management/configuration/guide/n1000v_sys_manage/system_6ntp.pdf
https://www.cisco.com/c/dam/en/us/td/docs/ios-xml/ios/bsm/configuration/xe-3se/3650/bsm-xe-3se-3650-book.html#GUID-A1071998-72BE-4F2E-8BC0-3A9FDC5D67EE
