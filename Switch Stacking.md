# Switch Stacking/Chassis Aggregation

A stack is a network solution composed of two or more stackable switches. Switches that are part of a stack behave as one single device. As a result, a stacking solution shows the characteristics and functionality of a single switch, while having an increased number of ports.

Allows for one configuration to be used across all switches within a stack, and appear as a single switch to the rest of the network. Telnet or SSH to one switch with a single management IP address. STP,CDP,VTP are running on one switch. The ports on each physical switch appear to be part of the same logical switch, and it uses one MAC address table. Creating one Single Virtual Switch.

For example instead of setting up STP between 4 switches and have one port blocking, and one forwarding for each connection to the distribution layer; with all switches connected to eachother in a spine leaf architecture. You would have those ports in a Etherchannel group instead because it is now one logical switch.

# A few protocols used for stacking are:

**Stackwise**: used on older devices like 3750

**Cisco's Flex Stack**: older version of flexstack supported on 2960s, 2960r series switches, and some other models.

**FlexStack Plus**: A newer version of flexstack supported on 2960x,2960xr

Uses Stacking Cables where the switches are connected in a series/ring. Where the last switch in the stack is connected back to the first switch using full duplex.

All Cisco Business stacks have an Active switch, or commander. The Active switch is a switch in the stack that handles the configuration for the entire stack. When you want to manage your stack, the Active switch is the device that you connect to in order to make changes. The Active switch also handles other important stack functions, such as detecting when switches enter or leave the stack, and upgrading outdated switches.

A **Standby switch**- is a switch that will become the new Active switch if the original Active switch goes offline. In this way, a backup helps maintain the resiliency of the stack.

A **Member**- is a stackable switch that operates as an additional unit within the stack.

A **stack port**- is a port on the switch that is used to communicate with other switches in the stack. Depending on the model, a switch can have either preconfigured or user-defined stack ports.

**Flexstack**: switch 2960s, 2960x. Speed full duplex is 10gbps. Supports up to 4 switches in one stack.
**Flexstack-Plus**: switch 2960-x, 2960-xr. Speed full duplex is 20gbps. Supports up to 8 switches in one stack. 

# Chassis Aggregation:

Switch stacking is usually used at the access layer, whereas chassis aggregation is used on more powerful equipment in the distribution/core layers. Chassis based switches does not require special hardware adapters, but rather uses ethernet interfaces. Usually only uses 2 switches. Same idea as switch stacking. 
