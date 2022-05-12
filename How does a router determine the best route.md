# How does a router make routing decisions

Router makes routing decisions based upon what is in it's FIB/Route table, or Forwarding information base. 

**If there is more than one entry for an IP address it first decides to take the path with the longest network match.** 
**If they are equal then it makes decisions based upon administrative distance.** 

For Example:
If I wanted to go to 10.1.1.2/24 and there were multiple routes in the table: 10.0.0.0/8, 10.1.0.0/16 and 10.1.1.0/24 and 10.1.1.0/30 it would take the /30 route because that has the longest network match, because it has 30 network bits. Similarly if the only path is 10.0.0.0/8 and there are multiple routing protocols giving different paths it would go by the lowest administrative distance.

# Admin Distance:
<table>
<tr>
    <th style = style = "text-align: center"> Protocol</th>
    <th style = style = "text-align: center"> Administrative Distance</th>
</tr>
<tr>
    <td>Connected:</td> 
    <td style = "text-align: center">0</td>
</tr>
<tr>
    <td>Static:</td> 
    <td style = "text-align: center">1</td>
</tr>
<tr>
    <td>EBGP:</td>
    <td style = "text-align: center">20 (exterior)</td>
</tr>
<tr>
    <td>EIGRP:</td>
    <td style = "text-align: center">90</td>
</tr>
<tr>
    <td>OSPF:</td>
    <td style = "text-align: center">110</td>
</tr>
<tr>
    <td>RIP:</td>
    <td style = "text-align: center">120</td>
</tr>
<tr>
    <td>IBGP:</td>
    <td style = "text-align: center">200 (interior)</td>
</tr>
</table>

------------------------------------------------------------------------------------------------------------------------------------------------
# Notes: 

This is pretty basic, there are more verbose explanations, but this is the easiest way I could explain/understand it. 
