# Access Control Lists (ACL)
**Access control list**- Access control lists (ACLs) perform packet filtering to control which packets move through a network and to where. The packet filtering provides security by helping to limit the network traffic, restrict the access of users and devices to a network, and prevent the traffic from leaving a network. IP access lists reduce the chance of spoofing and denial-of-service attacks and allow dynamic, temporary user-access through a firewall.

IP access lists can also be used for purposes other than security, such as bandwidth control, restrict the content of routing updates, redistribute routes, trigger dial-on-demand (DDR) calls, limit debug output, and identify or classify traffic for quality of service (QoS) features or what traffic to be encrypted when using a vpn. This module provides an overview of IP access lists.

*Note:**Inbound and Outbound traffic is based on the port it is being acted upon.** For example when making an ACL from a laptop going through a router, and to a server. The port that the laptop is connected to the router. Anything being sent from the laptop to that port would be inbound traffic, and anything coming out of that port to the pc would be outbound traffic. Same for the server side of this example. The port that the server directly connects to any traffic coming from the server into the router would be inbound, and anything being sent out of that port to the server would be outbound traffic. It is all based on the port you are putting the ACL on.* 

## 2 steps to implmenting ACLs
1. Create ACL in Terminal configuration mode: access-list command
2. Apply inbound or outbound on an interface: access-group command
An ACL will have no effect until it is applied to an interface. 

**ACL's are processed top down from the list**, and at the end if there are no matches in the ACL for traffic it is dropped. **At the end of ACL lists they have an Implicit DENY ALL**, so you *must have atleast one allow statement in an ACL or all traffic will be dropped*. You can **only have one ACL per interface, per direction, and per protcol. They get overwritten without warning**. 

**Standard ACL** - **Only check on source IP addresses** and do not check on port numbers or individual protocols. Permits or denies entire protocol suite based on the source Ip address or source network. These usually should be placed as close to the destination as possible.

**Extended ACL**- **Check on both the source and destination IP addresses**. Allows you to permit or deny specific protocols, ports and applications. These usually should be placed as close to the source as possible.

## Two methods to identify Standard and Extended ACLs:
<table>
<tr>
      <th>ACL</th>
      <th>Explanation</th>
</tr>
<tr>   
      <td VALIGN = "top"> Numbered ACL</td> 
      <td>1-99 and 1300-1999(expanded range) for standard IP ACLs.
      </br>100-199 and 2000-2699(expanded range) for Extended ACLs</td>
</tr>
<tr>
      <td>Named ACL</td> 
      <td>Uses Alphanumeric characters, and are more descriptive.</td>
</tr>
</table>

*Note: there is a 700-799 range for MAC address access list not covered in course, but may be something to look into later.*

# Wildcard Masks:

A wildcard mask is the opposite of a subnet maks where it uses 1's for the network portion. Wildcard masks instead use 1's for the host portion of the address or the 0's in a binary representation of a subnet mask. So whereas a /24 subnet mask is 255.255.255.0, a wildcard for /24 would be 0.0.0.255.
You can calculate a wildcard mask by subtracting the subnet from 255.255.255.255. To match a specific host you would use 0.0.0.0 which is the same as a all 255 subnet mask or /32, and to match anything would be 0.0.0.0 255.255.255.255. To match a subnet you would use the wildcard mask example below to figure out what you would need and a network address.</br>
```
For example 10.1.1.0 0.0.0.255, which would match the entire 10.1.1.0/24 network.
so as in our /24 example
  255.255.255.255
- 255.255.255.0
--------------------
   0.0.0.255 = Wildcard mask for a /24 network
```
**An interesting thing to note is that you can also use a wildcard mask to deteremine if two IP addresses are in the same subnet, identify IP addresses in a particular subnet, and Ip addresses that are discontiguous or not all grouped sequentially.**

# Configuring Wildcard Masks for Specific IP ranges:

So for specific wild card masks would be the same as a /32 address, so it would be ip address followed by 255.255.255.255 , and for entire subnets it would be 255.255.255.255 subtracted by the subnet mask of the network you are tyring to block.</br>
For specific ranges on the other hand we would need a different method of finding our wildcard mask. You would take the address range and take the lowest address and apply the ip subnet mask that applies to that range to figure out the specific address wildcard mask.
```
 For example you want 192.168.0.10 - 192.168.0.15 blocked
 That is 6 IP address to be blocked. 
 You would choose a /29 subnet mask.
   255.255.255.255
 - 255.255.255.248
 ------------------
     0.  0.  0.  7

  The resulting range would be 192.168.0.10  with a wildcard mask of 0.0.0.7
```
***Note: In the above example the /29 subnet mask blocks 8 addresses starting with 192.168.0.10, you would either need an allow statement before the block statement that allows the last two IP addresses(192.168.0.16-17) or those addresses will be blocked as well.***

# Setting up ACL

<table>
<tr>
    <th style = "text-align: center">configuration:</br> Terminal | Interface | Line</th>
    <th style = "text-align: center"> Commands </th>
    <th style = "text-align: center"> Explanation</th>
    <th style = "text-align: center"> Examples </th>
</tr>
<tr>
      <td VALIGN = "top">config#</td>
      <td VALIGN = "top">access-list [number] {deny|permit|remark}</td>
      <td VALIGN = "top">Creates a numbered access list and then chooses what to do with the traffic</td>
      <td>access-list 1 permit 10.1.1.1 0.0.0.0 (sets a standard acl to permit 10.1.1.1 when applied to interface.)
      </br>access-list 1 remark allow traffic from 10.1.1.1 out of interface. (allows a note in access list)
      </br>
      <list>
            <li>Deny- Denies traffic within ACL</li>
            <li>Permit- Permits traffic within ACL</li>
            <li>Remark- Sets a description for the ACL so you know what it's for if visiting it at a later date.</li>
      </list>
      </td>
</tr>
<tr>
      <td VALIGN = "top">config#</td> 
      <td VALIGN = "top">access-list {number|name} {deny|permit} <protocol> <source ip> <wildcard mask> [ports] <destination> [ports] [<options>]</td>
      <td VALIGN = "top"> creates an extended ACL that allows for more control than the standard ACL
      <td> access-list 100 permit TCP 10.1.1.1 0.0.0.0  192.168.1.1 eq 80(sets an extended acl to allow tcp http traffic from 10.1.1.1 to 192.168.1.1.we know its http traffic because of the equals port 80 command)
      </br>access-list 100 deny ip 10.1.1.0 0.0.0.255 host 192.168.1.1 (sets an extended acl that will block any ip traffic coming from the 10.1.1.0/24 network to 192.168.1.1. Because there is a permit message before this one, only the http traffic of 10.1.1.1 address would be allowed through all other traffic is dropped.)
      </td>
</tr>
<tr>
      <td VALIGN = "top">config-if#</td>
      <td VALIGN = "top">ip access-group {number|name} {in|out}</td>
      <td VALIGN = "top"> sets the ACL on the port to be implemented on inbound or outbound traffic</td>
      <td Valign = "top"> access-group 100 out (sets access list 100 to be set for traffic coming our of port configured)
      </br>access-group 1 in (sets access list 1 to be set for traffic coming into port configured)
      </td>
</tr>
<tr>
      <td Valign = "top">config#</td>
      <td Valign = "top">access-list {number|name} {deny|permit|remark} {hostname or ipaddress|any|host} {wildcard mask|log|ip address if host was used}</td>
      <td Valign = "top">host allows for you to match a specific IP and not use a wildcard mask.</td>
      <td Valign = "top"> access-list 1 permit host 10.1.1.1
      </br>access-list 1 permit 10.1.1.1 255.255.255.255
      </br>access-list 1 deny any log (denies all other traffic, and logs dropped traffic. The implicit deny does not log anything.)
      </td>
</tr>
<tr>          
      <td Valign = "top">config#</td>
      <td Valign = "top">access-list {number|name} {deny|permit|remark}</td>
      <td Valign = "top"></td>
      <td Valign = "top"> </td>
<tr>
      <td Valign = "top">config-if#</td>
      <td Valign = "top">ip access-group {name|number} {in|out}</td>
      <td Valign = "top"> Assign an access group/list to a specific port and set it for incoming or outgoing traffic</td>
      <td Valign = "top">ip access-group 1 in 
      </br>(This is the group you would assign to the interface and all ACL's tied to that group would be put on that interface. This is also what I was talking about before with the direction being applied to the port itself. The access list in this example would only apply to traffic coming into that port, so any end device sending traffic into the port to be distributed, whereas out would be any traffic going out of that port towards whatever endpoint/device is on the other end of the line.)
      </td>
</tr>
</table>
</br>

# Setting access list on VTY lines:

<table>
<tr>
    <th style = "text-align: center">configuration: Terminal | Interface | Line</th>
    <th style = "text-align: center"> Commands </th>
    <th style = "text-align: center"> Explanation</th>
    <th style = "text-align: center"> Examples </th>
</tr>
      <td Valign = "top">(config)#
      <td Valign = "top">access-list {number|word} {deny|permit} <ip address>
      <td Valign = "top">
      <td Valign = "top">(config)#line vty 0 4
      </br>(config-line)# access list 1 deny 192.168.1.10
</tr>
<tr>
      <td Valign = "top">config-if#</td>
      <td Valign = "top">access-class {number|word} {in|out}</td>
      <td Valign = "top"></td>
      <td></td>
</tr>
</table>

 # Removing a specific entry in an ACL:

<table>
<tr>
    <th style = "text-align: center">configuration: Terminal | Interface | Line</th>
    <th style = "text-align: center"> Commands </th>
    <th style = "text-align: center"> Explanation</th>
    <th style = "text-align: center"> Examples </th>
</tr>
<tr>
      <td Valign = "top">config#</td>
      <td Valign = "top">ip access-list {standard|extended} {name|number}
      </br>(config-acl)#no [number of entry you want deleted]
      </br>(config-acl)#do show ip access-list
      </td>
      <td Valign = "top"> explanation</td>
      <td Valign = "top">ip access-list standard 1
      </br>no 30
      </br>do show ip access-list
      </td>
</tr>
</table>

# Troubleshooting/Show Commands:
``show access-lists``</br>
``show access-lists {number|name}``
