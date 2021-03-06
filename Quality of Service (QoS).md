# Quality of service (QoS)

Is the description or measurement of the overall performance of a service, such as a telephony or computer network or a cloud computing service, particularly the performance seen by the users of the network. To quantitatively measure quality of service, several related aspects of the network service are often considered, such as packet loss, bit rate, throughput, transmission delay, availability, jitter, etc.

In the field of computer networking and other packet-switched telecommunication networks, quality of service refers to traffic prioritization and resource reservation control mechanisms rather than the achieved service quality. Quality of service is the ability to provide different priorities to different applications, users, or data flows, or to guarantee a certain level of performance to a data flow.

Quality of service is particularly important for the transport of traffic with special requirements. In particular, developers have introduced Voice over IP technology to allow computer networks to become as useful as telephone networks for audio conversations, as well as supporting new applications with even stricter network performance requirements.

Prioritizes traffic that can't handle delay. So things like video or voice traffic. It set's it so that traffic has priority over things that can handle delay like ftp, or http traffic. 

# 3 types of traffic we need to know for CCNA:

**Data**: Tends to be bursty/smooth, and greedy/benign. Meaning that if you give it 4mbps it will often use all bandwidth depending on the type of data traffic. It's generally drop-insensitive, as in if a packet is dropped it won't matter because of things like tcp. It's generally delay-insensitive, as in if a packet is delayed it won't hurt anything it just means that particular thing takes a bit longer to load/download, but that depends on the type of data sent.

2 major categories of Data applications- 
        
- Interactive Data applications- telnet, gaming, ssh. Where a delay will degrade the user experience.

- Non-Interactive Data Applications- Things like FTP, HTTP. Where a small delay will not matter much, it may take a webpage a small amount of time longer to load, but not a giant deal, generally speaking. 

**Voice**: Tends to be smooth, meaning that they require a set amount of bandwidth and require it for the duration of the call. Benign as in they will not try to grab bandwidth from other applications. They are very Drop sensitive and Delay Sensitive, because a delay in a voice call can cause degraded user experience, and packets cannot be retransmitted/played out of order for voice traffic. Voice is sent over UDP and has these one way requirements: Latency- less than 150mms, Jitter-less than 30ms, and Loss-less than 1%. Bandwidth required depends on codec used, but is usually between 30-128Kbps.

**Video**: Has characteristics of both Voice and Data. It is bursty/greedy like Data, and videos with alot of movement requires more bandwidth than one without alot of movement. It's drop/delay sensitive like Voice and transmits over UDP. Video traffic has these one way requirements: Latency- less than 150ms, Jitter-less than 30ms, loss- between 0.1%-1%, and bandwidth depending on the resolution of the video traffic may require 384Kbps-20+Mbps.

Transmission quality of the network is determined by Loss, Delay, and Delay variation(jitter).

# Terminology:

**Loss**- A relative measure of the number of packets that were not received compared to teh total number of packets transmitted.

**Delay**- the finite amount of time it takes a packet to treach the receiving endpoint after being transmitted from the sending endpoint. 

**Delay variation(jitter)**- The difference in the end-to-end delay between packets.

# Tools used for QoS:

**Classification and Marking Tools**- Looking at traffic types and putting them into classes and marking them. Either marks at layer2 802.1q header, or layer3 ToS or type of service which uses 8bits. Ip precedence uses the first 3 bits, or alternatively DSCP uses the first 6 bits, IP ECN(Explicit Congestion Notification not covered in CCNA) uses the last 2 bits of the ToS byte.

**Policing and Markdown tools**- If you are sending to much traffic it can be dropped, or the QoS given to you could be lowered.

**Scheduling tools (queuing and dropping)**- Schedules packets to be sent if traffic is high, and what traffic needs to be sent next, but not high enough to drop said packets. Classes are platinum, gold, silver, bronze, and CoS is 0,1,2,3,4,5,6,7. 5 for cisco voice traffic but may differ between vendors. This is why trunks use 802.1q encapsulation and why you may want to use a layer 3 marking, because layer 2 is not end to end. So if you go over a connection that is not a 802.1q your markings may be lost at layer 2.
At layer 3 in the IPv4 header there is a value called ToS (type of service) that consists of 8 binary bits and go from 0-7. 
Differentiated Services Code Point (DSCP) is a means of classifying and managing network traffic and of providing quality of service (QoS) in modern Layer 3 IP networks. It uses the 6-bit Differentiated Services (DS) field in the IP header for the purpose of packet classification. Note classes weren't standardized.

The Differentiated Service (DiffServ) is a network solution aimed at classifying the IP traffic flow  into traffic classes. It uses six bits, called DiffServ Code Point (DSCP), part of the eight-bit field called Type of Service (TOS) inside the IP header.

When 3 bits are used it's called Ip precedence, and when 6 bits are used it's called DSCP. DSCP is backwards compatible with ip precedence.

# Congestion management or scheduling tools

**Link-specific tools**- Link Fragmentation and interleaving

There is a trust boundry within a network to decide whether to trust the tags on packets, and that needs to be set on each device in the line. So the interface the phone is on, the swtich between that and the router, and so on. Alternatively you can set devices to trust all ingress traffic on a port.

**CoS**- Class of service

# Layer 3 packet header:

If the ToS is set to </br>

000 000 = best effort</br>

001 000 = ip precedence 1/ class selector 1</br>

**Assured forwarding classes**: Ranges from AF1 - 4 and the number indicates the first 3 binary bits. The following 3 binary bits set how likely it is for the packet to be dropped. The higher the number the more likely to be dropped. 
Each class has 3 subclasses for example of AF1

001 010 = AF11 - during times of network congestion this would be dropped third. This would be the same for AF21,AF31,AF41 and so on.

001 100 = AF12 - during times of network congestion this would be dropped second

001 110 = AF13 - during times of network congestion this would be dropped first

The higher the first number in AF the more important your traffic, and they higher your second number the more likely it is for traffic to be dropped. So AF411 is more important than AF11. 

# Qos Precedence Level:
## 7- Stays the same (link layer and routing protocol keep alive)
## 6- Stays the same (used for IP routing protocols)
## 5- Express forwarding-DSCP value of 46
## 4- Class 4
## 3- Class 3
## 2- Class 2
## 1- Class 1 
## 0- Best Effort

# Drop Probablities for Assured Forwarding

 The number next to the AF value is it's 6 digit binary representation in decimal or DSCP

**Low**- AF11-10,AF21-18,AF31-26,AF41-34

**Medium**- AF12-12,AF22-20,AF32-28,AF42-36

**High**- AF13-14,AF23-22,AF33-30,AF43-38

Expidited forwarding is built for low loss, low latency, jitter, assured bandwidth, end-to-end service through DS(diffeserv) domains. Codepoint is 101 110 or DSCP 46. Generally used for voice traffic. Voice traffic would set it's DSCP to 101 110 and its CoS to 5

# Matching all voice traffic with DSCP or setting classes/priority:
```
(config)#class-map {word|match-all|match-any|type} <class-map name>
      example: class-map match-any VOIP
      
      word: class-map name
      match-all: Logical-AND all matching statements under classmap-All statments must be true to match
      match-any: Logical-or all matching statements under this class map- Any of the statements must be true to match
      type: configure CPL class map

(config-cmap)#match ip dscp <0-63>
      example: match ip dscp 46 <ef> (could also use ef instead of the dscp value. Match has a ton of values after it doesn't have to be ip we just used it for this value. You should use the ? after match to see values and go from there.)

(config-cmap)#do show run | include class-map or do show run to verify configuration      
      
(config-cmap)#match ip precedence <0-7> (can use number or name of precedence)
      example: matche ip precedence 5
      <0-7> Enter up to 4 precedence values separated by white-spaces
      network-7
      internet-6
      critical-5
      flash-override-4
      flash-3
      immediate-2
      priority-1
      routine-0
      
(config-cmap)# match application <options> (many options use ? to view them all)

(config-cmap)#match mpls experimental topmost <0-7> (matches by mpls)
      <0-7> enter up to 8 experimental values separated by white-spaces
```     
Much like a ACLs after a Class-Map is made and you have matches set you can decide what to do with the traffic, like drop, forward, queue, etc. 


**Trust Boundry**- Is a boundry where switch/router devices trust QoS packet information. For example a switch may not trust a QoS marking from pc, but would trust it from an IP phone. What to trust must be set up on the device, and every device down the line.

**Untrusted Domain**- is the part of the network that you are not managing. Would be a PC or a Printer. 

**Trusted Domain**- is the part of the network that only administrators can manage. Would be routers, switches, and in some cases ip phones.

A router will trust the markings that come from a switch, and vice versa. The trust boundry is where the traffic is marked. ISP's however may not trust markings sent by customers, so that would be a different trust boundry. By default cisco routers will override any QoS markings they receive from an untrusted boundary.

Before QoS can be applied the router or switch must determine the class the traffic belongs to, and different classes have different service.

# Best practices for Qos:
  
Classify and mark as close to the edge of the network as possible. So ip phones would mark its traffic as it leaves the phone, and other devices would be classified/marked on the edge switches/routers. Marking takes place on the edge, but all devices along the path use classification to determine QoS that traffic gets. 

# Classification is based upon 3 criteria:

**Marking**- COS or DSCP value

**Ip Addressing**- destination ip subnet, specific address, layer w mac address, specific port number, domain address, etc. 

**Deep Packet Inspection**- NBAR network based application recognition.

## Resources:

SRND guide:</br>
https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&cad=rja&uact=8&ved=2ahUKEwjNla__i9L0AhXckIkEHQsHDtUQFnoECAoQAQ&url=https%3A%2F%2Fsupportforums.cisco.com%2Flegacyfs%2Fonline%2Flegacy%2F3%2F2%2F6%2F64623-qossrnd.pdf&usg=AOvVaw1Vntqy6r25GAhLzDe-XMQE