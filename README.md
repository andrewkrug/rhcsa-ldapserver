#Vagrant File for CentOS 7 with LDAP Server

This vagrant box is designed to perform the function of LDAP server for learning to connect an LDAP client.

**The LDAP Server is configured with the LDAP Base:**

    DC=thorin,DC=co

The following users have been provisioned in the LDAP directory:

| Username | PW |
| :------- |:-- |
|bilbo |infosec|
|thorin|infosec|
|kili|infosec|
|fili|infosec|
|ori|infosec|
|nori|infosec|

If you are asked for a password the password is Vagrant.

This vagrant file also attempts a bind to port 389 on the host system.  You may need to change this.
