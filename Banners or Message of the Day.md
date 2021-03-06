# Banners and Message of the Day MOTD

## Different types of Banners:
- Line- c banner-text c, where 'c' is a delimiting character
- exec- set exec process creation banner
- incoming- set incoming terminal line banner
- login- set login banner
- motd- set message of the day banner
- prompt-timeout- set message for login authentication timeout
- slip-ppp- set message for slip/ppp
</br>

*Note:These banner types depeond on the device and what banner types it supports. Not all banner types are supported across all devices.*

**Message of the day banner** - To display a temporary message (Is displayed before the password prompt. Displays on all lines- console, aux, and tty/vty lines)</br>
**Login Banner**- Is shown before a user logs in (Only shows if login is enabled. For example it will show up if you telnet in and login is enabled, but not if login is not enabled for console and so forth. Unauthorized Access Prohibited)</br>
**Exec Banner**- Displays after Login (Display information ony internal staff should know. Only displayed after successful authentication.)

# Configuring banner
```
(console)#config t
(config)#banner @ <message> @ (You could use C like it says, but if your message has C in it, it will be cut off. Use a delimiter not in your Message like @ or $.) 
```

*Note: TTY lines are used for modem and terminal connections. (Physical connection like if you had an old phone modem hooked up and dialed into the device)*</br>
*VTY lines are used for inboud telnet/ssh connections. (Are for virtual connections like when you telnet/ssh to a devices ip)*
