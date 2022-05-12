# Backing up an IOS image to TFTP:

1. Verify connectivity to server and that it has enough space to host the file. 
2. on device do a show flash command to copy filename wanted to transfer to TFTP server.
    example: R1#show flash
3. copy flash: <filename> tftp: or copy flash: ? (will bring up options so you can choose what file to copy) tftp:
4. add ip address of tftp server. it will ask you what you want destination filename to be. 



# Downloading/Upgrading and IOS imaging from one that has been downloaded to your TFTP server:

1. Verify connectivity to tftp server and enough space on device to host file
    </br>example: R1#show flash (verify device has enough space to host file) 
    </br>ping <\tftp server address> (verify connectivity)
2. Copy tftp: <\source file name> flash: or copy tftp: flash: ( it will ask for the address of tftp, source filename, and destination filename)

# Booting from a different system image:

1. Enter privledged exec mode aka configure terminal
2. (config)boot system {flash | ftp | mop | rcp | rom | tftp} <\filename>
    </br>example: boot system flash: <\filename>
    </br>flash-boot system from flash memory
    </br>ftp- boot system from a server via ftp
    </br>mop- boot from a Decnet MOP server
    </br>rcp- boot from a server via rcp
    </br>rom- boot from ROM
    </br>tftp- boot from a tftp server


**Configuration register 0x2142 is to boot up skipping start up config and 0x2102 is for normal boot.**

# Changing forgotten/lost secret password:

Break boot and enter ROMmon mode. Change console register to 0x2142 by using the confreg command. After booting up machine say no to enter initial config, and then copy startup config to running config. Change secret password, save running to startup config, and then change register to 0x2102 to put it back to boot normally. 

# To put the router/switch back into/skip normal boot mode before/after changing a secret password:
```
rommon 1>confreg 0x2142
(config)# copy start run
(config)#enable secret <password> (overwrites previous secret password)
(config)#copy run start or wr
(config)#config-register 0x2102
```

# An example of password recovery, but it depends on device you are using and a password decryptor for type 7 passwords: 

http://learnccna.net/cisco-type-7-password-decryption-2/73D9CD6E4B859E7B0978A7378A959B6890EF0DE9A93FA5122B6B10F5ED5122C8CD0583C3AA79253EBC64724A2B2655E3/

https://www.cisco.com/c/en/us/support/docs/routers/2800-series-integrated-services-routers/112033-c2900-password-recovery-00.html
