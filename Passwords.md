# To create a privledge mode password:
```
(config)#enable secret <password>

or 

(config)#enable password <password-Level 0 or 7> <password> 
(0 is unencrypted password and 7 specifies an encrypted password, default is 0)

(console)#service password-encryption
```

# To create console password:
```
(config)#line con 0
(config-line)#enable secret <password>

or

(config)#line con 0
(config-line)#enable password <password> (unencrypted)
(config)#service password-encryption
```

# Create username and password pairs for login:

This creates a local user/password on the router/switch, and allows you to set login options.
```
(config)username <name> [privledge]{level 0-15} {password|secret} [encryption-type] <password> 
( You can set both privledge and encryption type, but I would not do that. If setting encryption type use 0 for unencrypted and 7 for a hidden password.)

Example: username admin secret password123 or username admin password password 123
```
```
(config)#line console 0
    
or

(config)#line vyt {0-whatever number the switch/router has for vty lines}

(config-line)#login local (sets switch to look for login information locally on the device)

or

(config)#line con 0
(config)#password [level 0 or 7]<password> ( sets password on the line and does not require a username. If level not chosen default is 0 and will need to use service password-encryption. If set up this way needs to be set up on each line you use either console for vty.) 
```
***Note: </br>If you set a login local for line con 0 and exit all the way out of the switch without adding username password pairs you will no longer be able to log in. You will either need to use password recovery, wipe the device and start again, or use the configure register 0x2142 trick documented in the section Backup IOS to TFTP; if you have already saved to startup config.***

# Remove a password:
```
(config)#no enable secret
(config)#no enable password
```