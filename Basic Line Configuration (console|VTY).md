# Basic Line configuration for both Console and VTY lines.
These are some line configurations with passwords and such so that I don't forget. 
## Console:
```
(console)#config t
(config)#username <username> secret <password>
(config)#enable secret <password> ( sets password for privledged exec mode or enable mode)
(config)#line con 0
(config-line)#login local (sets login for the console so before you get to the enable portion)
(config-line)#exec-timeout <seconds> (time you have to put in password before timeout) 
```
## Virtual TeleType VTY:
```
(console)#config t
(config)#enable secret <password> ( sets password for privledged exec mode or enable mode)
(config)#username <username> secret <password>
(config)#line vty <starting vty number> <ending vty number> (you could do specific lines by choosing just one, or a range from start to end.  Like, 0, with a space between, then the last line you want, like 4; to configure all vty at once.)
(config-line)#login local
(config-line)#transport input {all|none|telnet|ssh} (What protocol to user for incoming connections to terminal. Telnet is clear text transportation should always use SSH if possible.)
(config-line)#transport output {all|none|telnet|ssh} (what protocl to use for outgoiong connections so from local device to a different one)
```
# Transport types:
Telnet-clear text</br>
SSH- encrypted and requires crypto key gen rsa general-key modulus </br>
none- no transport protocols accepted. Basically disables the lines</br>
ALL- allows all transport protocols: lat|mop|nasi|pad|rlogin|ssh|telnet|v120</br>


# Setting up VTY for SSH specifically:
```
(console)#config t
(config)#hostname <name>
(config)#ip domain-name <name>
(config)#enable secret <password> (need to have enable password for both telnet or SSH to work)
(config)#username <username> secret <password>
(config)#crypto key generate rsa general-keys modulus <360-4096> (You need to have a hostname and domain name configured for this. It is what creates the keys for the ssh session.)(creates public and private keys alternatively I tested on a router and when I pressed enter after rsa it asked me for the modulus and skipped the general keys part.)
(config)#line vty <starting vty> <ending vty>
(config-line)#login local
(config-line)#transport input ssh version 2 (version is optional, but ssh version 2 is more secure)
(config-line)#no shut
```
# Commands to ssh to device

``ssh -l(the L is for login) <username> <ip address/hostname> `` 
