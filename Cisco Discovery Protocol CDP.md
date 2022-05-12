# Cisco Discovery protocol (CDP)
CDP is a Cisco proprietary protocol that is used for collection directly connected neightbor device information like hardware, software, device name details, etc...

## How CDP Works:
All Cisco devices transmit CDP packets periodically (default time interval value is 60sec). These packets advertise a time-to-live(TTL) value in seconds, which indicates the number of seconds that the packet must be retained before it can be discarded (default value is 180sec)

CPD packets are sent with a TTL value that is nonzero after an interface is enabled and with a ttl of zero immediately before an interface is down. This provides quick state discovery.

All Cisco devices receive CDP packets, process them and chache the information in the packet. Cisco devices never forward a CDP packet. If any information changes from the last received packet, the new information is cached and the older information is discarded even if it's TTL value has not yet expired.

# Notes for CDP:

CDP only works on directly connected interfaces.

CDP messages are generated every 60second, hold-down timer is 180sec

Messages are destined to L2 Multicast address 01:00:0c:cc:cc:cc

Devices that receive CDP messages on an interface from other devices the information is stored in a table that can be viewd using the show CDP neighbors command.

CDP table information is refreshed each time an announcment is received from neighbor, and the holdtime for that entry is reinitialized.
The holdtime specifies the lifetime of an entry in the table - if no announcements are received from a device for a period in excess of the holdtime, the device information is discarded and wiped from the table

CDP runs on all media that support Subnetwork Access Protocol (SNAP), including LAN, Frame Relay, and Asynchronous Transfer Mode(ATM) physical media.

CDP runs over the data link layer only. Therefore, two systems that support different network-layer protocols can learn about each other.

CDP Version-2 (CDPv2) is the most recent release of the protocol and provides more intelligent device tracking features. These features include a reporting mechanism, which allows more rapid erro checking, therby reduces costly downtime. Errors reported inclued: Mismatched native VLAN IDs on connected ports and Mismatched Port-duplex states between connected devices.

CDP can be enabled on GRE tunnel wich is useful in DMVPN. A central hub can use "router odr" to insert a default route into the spoke so spoke can route viea the hub. in addition odr can be redistributed to other routing protocols. Finally show cdp entry * pro can show all the IPs of connected devices. CDP support on GRE tunnel interfaces was integrated 12.3.5

CDP is used in output power negotiations for POE capable devices; like IP phones, access points etc. 

CDP Frame Format:
CDP is assigned HDLC protocol type value of 0x2000. A cisco-proprietary SNAP value enumerates HDLC protocol type values so CDP can run on all media that support SNAP, such as LAN media, Frame Relay, and ATM

SNAP-The SNAP feature on the Cisco IAD2420 series IAD allows service providers to rapidly deploy and configure services to the Cisco IAD platforms at customer premises without requiring configuration of the IADs at the customer site and with little or no onsite technician intervention. SNAP is part of the Cisco Networking Services (CNS) technology, which allows network products to be installed and automates many of the configuration tasks. SNAP consists of two basic functions: learning and setting the IP address and downloading the configuration for the IAD.

# Terminology: 
**HDLC**-  is an extension to the High-Level Data Link Control (HDLC) network protocol, and was created by Cisco Systems, Inc. HDLC is a bit-oriented synchronous data link layer protocol that was originally developed by the International Organization for Standardization (ISO). Often described as being a proprietary extension, the details of cHDLC have been widely distributed and the protocol has been implemented by many network equipment vendors. cHDLC extends HDLC with multi-protocol support.

**Frame Relay**-  is a standardized wide area network (WAN) technology that specifies the physical and data link layers of digital telecommunications channels using a packet switching methodology. Originally designed for transport across Integrated Services Digital Network (ISDN) infrastructure, it may be used today in the context of many other network interfaces.

Network providers commonly implement Frame Relay for voice (VoFR) and data as an encapsulation technique used between local area networks (LANs) over a WAN. Each end-user gets a private line (or leased line) to a Frame Relay node. The Frame Relay network handles the transmission over a frequently changing path transparent to all end-user extensively used WAN protocols. It is less expensive than leased lines and that is one reason for its popularity. The extreme simplicity of configuring user equipment in a Frame Relay network offers another reason for Frame Relay's popularity.

With the advent of Ethernet over fiber optics, MPLS, VPN and dedicated broadband services such as cable modem and DSL, Frame Relay has become less popular in recent years.

**ATM**- ATM is a cell-switching and multiplexing technology that combines the benefits of circuit switching (guaranteed capacity and constant transmission delay) with those of packet switching (flexibility and efficiency for intermittent traffic). It provides data link (Layer 2) services with scalable bandwidth from a few megabits per second (Mb/s) to many gigabits per second (Gb/s), which usually run over SONET/SDH physical (Layer 1) links.

ATM networks consist of ATM switches interconnected by point-to-point ATM links or interfaces and are fundamentally connection oriented, which means that a virtual channel (VC) must be set up across the ATM network prior to any data transfer.


# CDP Configuration:
```
(config)# cdp run (Enables CDP globally on device.)
(config)#no cdp run (Disables CDP globally on device)
(config)#cdp enable (Enables CDP on an interface device if CDP is enabled globally)
(config)#no cdp enable (Disables CDP on an interface device)
(config)#cdp timer <seconds> (Specifies CDP packets transmission frequency. Default 60sec.)
(config)#cdp holdtime <seconds> (Specifies time limit for which a receiving device should hold information before discarding. Default 180sec.)
```
***Things to Note:***
</br><i>CDP is enabled by default on Cisco Devices</i>
</br><i>Disabling CDP globally and enabling on an interface is not possible.</i>
</br><i>If on  and interface CDP is disabled and then the encapsulation of the interface is changed, CDP is re-enabled automatically on wthat interface even if CDP was previously installed.</i>

# Troubleshooting/Show commands:

``clear cdp counters``</br>
``clear cdp table``</br>
``show cdp``</br>
``show cdp entry device-name [protocol | version]``</br>
``show cdp interface [type number]``</br>
``show cdp neighbors [type number][detail]``</br>
``show cdp traffic``</br>

# CDP Spoofing: 
In CDP spoofing, an attacker sends packet with multicast mac-address (01:00:0c:cc:cc:cc) as destination and various spoofed or fake mac-addresses as source. When a Cisco Device receives these frames it starts to add the information in CDP table and the table will start to build larger because the attacker may sends thousands of CDP frames to the device. If the device is unable to handle this attack there is a probability that the device may crash after a few time that's why it is recommended to disable CDP on interfaces that connects non cisco devices , user station. **This would be a DOS attack.**

## Resources:

https://learningnetwork.cisco.com/s/article/cisco-discovery-protocol-cdp-x

