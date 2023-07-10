# 4. Initial Enumeration - Enumerating Using RPCLIENT

Status: Not started

### Using rpcclient

Using rpcclient

```
Foresty@htb[/htb]$ rpcclient -U "" -N 172.16.5.5
rpcclient 
$> querydominfo
Domain:		INLANEFREIGHT
Server:
Comment:
Total Users:	3650
Total Groups:	0
Total Aliases:	37
Sequence No:	1
Force Logoff:	-1
Domain Server State:	0x1
Server Role:	ROLE_DOMAIN_PDC
Unknown 3:	0x1

```

### Obtaining the Password Policy using rpcclient

Obtaining the Password Policy using rpcclient

```
rpcclient $> querydominfo
Domain:		INLANEFREIGHT
Server:
Comment:
Total Users:	3650
Total Groups:	0
Total Aliases:	37
Sequence No:	1
Force Logoff:	-1
Domain Server State:	0x1
Server Role:	ROLE_DOMAIN_PDC
Unknown 3:	0x1
rpcclient $> getdompwinfomin_password_length: 8
password_properties: 0x00000001
	DOMAIN_PASSWORD_COMPLEX

```