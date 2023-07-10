# 2. Initial Enumeration - Uncredentialed


1. **Enumerating Using NMAP**

   `sudo nmap -v -A -iL hosts.txt -oN /home/htb-student/Documents/host-enum`
1. **Kerbrute - Internal AD Username Enumeration**

   username wordlist : <https://github.com/insidetrust/statistically-likely-usernames>
   > Download Kerbrute, Untuk Compile nya liat di HTB Academy section Initial Enumeration

**Enumerating Users with Kerbrute**

1. Enumerating User With Kerbrute
~~~ bash
Foresty@htb[/htb]$ kerbrute userenum -d INLANEFREIGHT.LOCAL --dc 172.16.5.5 jsmith.txt -o valid_ad_users
2021/11/17 23:01:46 >  Using KDC(s):
2021/11/17 23:01:46 >   172.16.5.5:88
2021/11/17 23:01:46 >  [+] VALID USERNAME:       jjones@INLANEFREIGHT.LOCAL
2021/11/17 23:01:46 >  [+] VALID USERNAME:       sbrown@INLANEFREIGHT.LOCAL
2021/11/17 23:01:46 >  [+] VALID USERNAME:       tjohnson@INLANEFREIGHT.LOCAL
2021/11/17 23:01:50 >  [+] VALID USERNAME:       evalentin@INLANEFREIGHT.LOCAL

 <SNIP>

2021/11/17 23:01:51 >  [+] VALID USERNAME:       sgage@INLANEFREIGHT.LOCAL
2021/11/17 23:01:51 >  [+] VALID USERNAME:       jshay@INLANEFREIGHT.LOCAL
2021/11/17 23:01:51 >  [+] VALID USERNAME:       jhermann@INLANEFREIGHT.LOCAL
2021/11/17 23:01:51 >  [+] VALID USERNAME:       whouse@INLANEFREIGHT.LOCAL
2021/11/17 23:01:51 >  [+] VALID USERNAME:       emercer@INLANEFREIGHT.LOCAL
2021/11/17 23:01:52 >  [+] VALID USERNAME:       wshepherd@INLANEFREIGHT.LOCAL
2021/11/17 23:01:56 >  Done! Tested 48705 usernames (56 valid) in 9.940 seconds
~~~

\# 3. Initial Enumeration - Password Spraying - Enum User to Exploit



1. **Using enum4linux**


~~~ bash
Foresty@htb[/htb]$ enum4linux -U 172.16.5.5  | grep "user:" | cut -f2 -d"[" | cut -f1 -d"]"

administrator

guest

krbtgt

lab\_adm

htb-student

avazquez

pfalcon

fanthony

wdillard

lbradford

sgage

asanchez

dbranch

ccruz

njohnson

mholliday

~~~

1. **Using RpcClient**

~~~ bash

Foresty@htb[/htb]$ rpcclient -U "" -N 172.16.5.5

rpcclient

$> enumdomusers

user:[administrator] rid:[0x1f4]

user:[guest] rid:[0x1f5]

user:[krbtgt] rid:[0x1f6]

user:[lab\_adm] rid:[0x3e9]

user:[htb-student] rid:[0x457]

user:[avazquez] rid:[0x458]

~~~ 

1. **Using Crackmapexec**

~~~ bash

Foresty@htb[/htb]$ crackmapexec smb 172.16.5.5 --users

SMB         172.16.5.5      445    ACADEMY-EA-DC01  [\*] Windows 10.0 Build 17763 x64 (name:ACADEMY-EA-DC01) (domain:INLANEFREIGHT.LOCAL) (signing:True) (SMBv1:False)

SMB         172.16.5.5      445    ACADEMY-EA-DC01  [+] Enumerated domain user(s)

SMB         172.16.5.5      445    ACADEMY-EA-DC01  INLANEFREIGHT.LOCAL\administrator                  badpwdcount: 0 baddpwdtime: 2022-01-10 13:23:09.463228

SMB         172.16.5.5      445    ACADEMY-EA-DC01  INLANEFREIGHT.LOCAL\guest                          badpwdcount: 0 baddpwdtime: 1600-12-31 19:03:58

SMB         172.16.5.5      445    ACADEMY-EA-DC01  INLANEFREIGHT.LOCAL\lab\_adm                        badpwdcount: 0 baddpwdtime: 2021-12-21 14:10:56.859064

SMB         172.16.5.5      445    ACADEMY-EA-DC01  INLANEFREIGHT.LOCAL\krbtgt                         badpwdcount: 0 baddpwdtime: 1600-12-31 19:03:58

SMB         172.16.5.5      445    ACADEMY-EA-DC01  INLANEFREIGHT.LOCAL\htb-student                    badpwdcount: 0 baddpwdtime: 2022-02-22 14:48:26.653366

SMB         172.16.5.5      445    ACADEMY-EA-DC01  INLANEFREIGHT.LOCAL\avazquez

~~~

1. **Gathering Users with LDAP Anonymous**

~~~ bash

Foresty@htb[/htb]$ ldapsearch -h 172.16.5.5 -x -b "DC=INLANEFREIGHT,DC=LOCAL" -s sub "(&(objectclass=user))"  | grep sAMAccountName: | cut -f2 -d" "guest

ACADEMY-EA-DC01$

ACADEMY-EA-MS01$

ACADEMY-EA-WEB01$

htb-student

avazquez

pfalcon

fanthony

wdillard

lbradford

sgage

asanchez

dbranch

~~~ 

1. **Using windapsearch**

~~~ bash

Foresty@htb[/htb]$ ./windapsearch.py --dc-ip 172.16.5.5 -u "" -U

[+] No username provided. Will try anonymous bind.

[+] Using Domain Controller at: 172.16.5.5

[+] Getting defaultNamingContext from Root DSE

[+]	Found: DC=INLANEFREIGHT,DC=LOCAL

[+] Attempting bind

[+]	...success! Binded as:

[+]	 None

[+] Enumerating all AD users

[+]	Found 2906 users:

cn: Guest

cn: Htb Student

userPrincipalName: htb-student@inlanefreight.local

cn: Annie Vazquez

userPrincipalName: avazquez@inlanefreight.local

cn: Paul Falcon

userPrincipalName: pfalcon@inlanefreight.local

cn: Fae Anthony

userPrincipalName: fanthony@inlanefreight.local

cn: Walter Dillard

userPrincipalName: wdillard@inlanefreight.local

~~~ 

\# 4. Initial Enumeration - Enumerating Using RPCLIENT



\### Using rpcclient

Using rpcclient

~~~ bash

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

Server Role:	ROLE\_DOMAIN\_PDC

Unknown 3:	0x1

~~~ 

\### Obtaining the Password Policy using rpcclient

Obtaining the Password Policy using rpcclient

~~~ bash

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

Server Role:	ROLE\_DOMAIN\_PDC

Unknown 3:	0x1

rpcclient $> getdompwinfomin\_password\_length: 8

password\_properties: 0x00000001

DOMAIN\_PASSWORD\_COMPLEX

~~~

\# 5. Initial Enumeration - Enumerating Using enum4linux-ng



**Using enum4linux-ng**

~~~ bash

Foresty@htb[/htb]$ enum4linux -P 172.16.5.5<SNIP>

\==================================================

|    Password Policy Information for 172.16.5.5    |

\==================================================

[+] Attaching to 172.16.5.5 using a NULL share

[+] Trying protocol 139/SMB...

[!] Protocol failed: Cannot request session (Called Name:172.16.5.5)

[+] Trying protocol 445/SMB...

[+] Found domain(s):

[+] INLANEFREIGHT

[+] Builtin

[+] Password Info for Domain: INLANEFREIGHT

[+] Minimum password length: 8

[+] Password history length: 24

[+] Maximum password age: Not Set

[+] Password Complexity Flags: 000001

[+] Domain Refuse Password Change: 0

[+] Domain Password Store Cleartext: 0

[+] Domain Password Lockout Admins: 0

[+] Domain Password No Clear Change: 0

[+] Domain Password No Anon Change: 0

[+] Domain Password Complex: 1

[+] Minimum password age: 1 day 4 minutes

[+] Reset Account Lockout Counter: 30 minutes

[+] Locked Account Duration: 30 minutes

[+] Account Lockout Threshold: 5

[+] Forced Log off Time: Not Set

[+] Retieved partial password policy with rpcclient:

Password Complexity: Enabled

Minimum Password Length: 8

enum4linux complete on Tue Feb 22 17:39:29 2022

~~~

\# 6. Initial Enumeration - Enumerating Using ldapsearch



**Using ldapsearch**

~~~ bash

#IPPSEC HTB SAUNA

Foresty@htb[/htb]$ ldapsearch -h 172.16.5.5 -x -b "DC=INLANEFREIGHT,DC=LOCAL" -s sub "\*" | grep -m 1 -B 10

pwdHistoryLengthforceLogoff: -9223372036854775808

lockoutDuration: -18000000000

lockOutObservationWindow: -18000000000

lockoutThreshold: 5

maxPwdAge: -9223372036854775808

minPwdAge: -864000000000

minPwdLength: 8

modifiedCountAtLastProm: 0

nextRid: 1002

pwdProperties: 1

pwdHistoryLength: 24

~~~

\# 7. Credentialed Enumeration - from Linux



1. **CME - Domain User Enumeration**

~~~ bash

Foresty@htb[/htb]$ sudo crackmapexec smb 172.16.5.5 -u forend -p Klmcargo2 --users

SMB         172.16.5.5      445    ACADEMY-EA-DC01  [\*] Windows 10.0 Build 17763 x64 (name:ACADEMY-EA-DC01) (domain:INLANEFREIGHT.LOCAL) (signing:True) (SMBv1:False)

SMB         172.16.5.5      445    ACADEMY-EA-DC01  [+] INLANEFREIGHT.LOCAL\forend:Klmcargo2

SMB         172.16.5.5      445    ACADEMY-EA-DC01  [+] Enumerated domain user(s)

SMB         172.16.5.5      445    ACADEMY-EA-DC01  INLANEFREIGHT.LOCAL\administrator                  badpwdcount: 0 baddpwdtime: 2022-03-29 12:29:14.476567

SMB         172.16.5.5      445    ACADEMY-EA-DC01  INLANEFREIGHT.LOCAL\guest                          badpwdcount: 0 baddpwdtime: 1600-12-31 19:03:58

SMB         172.16.5.5      445    ACADEMY-EA-DC01  INLANEFREIGHT.LOCAL\lab\_adm                        badpwdcount: 0 baddpwdtime: 2022-04-09 23:04:58.611828

SMB         172.16.5.5      445    ACADEMY-EA-DC01  INLANEFREIGHT.LOCAL\krbtgt                         badpwdcount: 0 baddpwdtime: 1600-12-31 19:03:58

SMB         172.16.5.5      445    ACADEMY-EA-DC01  INLANEFREIGHT.LOCAL\htb-student                    badpwdcount: 0 baddpwdtime: 2022-03-30 16:27:41.960920

SMB         172.16.5.5      445    ACADEMY-EA-DC01  INLANEFREIGHT.LOCAL\avazquez

~~~

1. **CME - Domain Group Enumeration**

~~~ bash

Foresty@htb[/htb]$ sudo crackmapexec smb 172.16.5.5 -u forend -p Klmcargo2 --groups

SMB         172.16.5.5      445    ACADEMY-EA-DC01  [\*] Windows 10.0 Build 17763 x64 (name:ACADEMY-EA-DC01) (domain:INLANEFREIGHT.LOCAL) (signing:True) (SMBv1:False)

SMB         172.16.5.5      445    ACADEMY-EA-DC01  [+] INLANEFREIGHT.LOCAL\forend:Klmcargo2

SMB         172.16.5.5      445    ACADEMY-EA-DC01  [+] Enumerated domain group(s)

SMB         172.16.5.5      445    ACADEMY-EA-DC01  Administrators                           membercount: 3

SMB         172.16.5.5      445    ACADEMY-EA-DC01  Users                                    membercount: 4

SMB         172.16.5.5      445    ACADEMY-EA-DC01  Guests                                   membercount: 2

SMB         172.16.5.5      445    ACADEMY-EA-DC01  Print Operators                          membercount: 0

SMB         172.16.5.5      445    ACADEMY-EA-DC01  Backup Operators                         membercount: 1

SMB         172.16.5.5      445    ACADEMY-EA-DC01  Replicator                               membercount: 0

<SNIP>

SMB         172.16.5.5      445    ACADEMY-EA-DC01  Domain Admins                            membercount: 19

SMB         172.16.5.5      445    ACADEMY-EA-DC01  Domain Users                             membercount: 0

<SNIP>

SMB         172.16.5.5      445    ACADEMY-EA-DC01  Contractors                              membercount: 138

SMB         172.16.5.5      445    ACADEMY-EA-DC01  Accounting                               membercount: 15

SMB         172.16.5.5      445    ACADEMY-EA-DC01  Engineering                              membercount: 19

SMB         172.16.5.5      445    ACADEMY-EA-DC01  Executives                               membercount: 10

SMB         172.16.5.5      445    ACADEMY-EA-DC01  Human Resources                          membercou

~~~

1. **CME - Logged On Users**

~~~ bash

Foresty@htb[/htb]$ sudo crackmapexec smb 172.16.5.130 -u forend -p Klmcargo2 --loggedon-users

SMB         172.16.5.130    445    ACADEMY-EA-FILE  [\*] Windows 10.0 Build 17763 x64 (name:ACADEMY-EA-FILE) (domain:INLANEFREIGHT.LOCAL) (signing:False) (SMBv1:False)

SMB         172.16.5.130    445    ACADEMY-EA-FILE  [+] INLANEFREIGHT.LOCAL\forend:Klmcargo2 (Pwn3d!)

SMB         172.16.5.130    445    ACADEMY-EA-FILE  [+] Enumerated loggedon users

SMB         172.16.5.130    445    ACADEMY-EA-FILE  INLANEFREIGHT\clusteragent              logon\_server: ACADEMY-EA-DC01

SMB         172.16.5.130    445    ACADEMY-EA-FILE  INLANEFREIGHT\lab\_adm                   logon\_server: ACADEMY-EA-DC01

SMB         172.16.5.130    445    ACADEMY-EA-FILE  INLANEFREIGHT\svc\_qualys                logon\_server: ACADEMY-EA-DC01

SMB         172.16.5.130    445    ACADEMY-EA-FILE  INLANEFREIGHT\wley                      logon\_server: ACADEMY-EA-DC01

~~~

1. **Share Enumeration - Domain Controller**

~~~ bash

Foresty@htb[/htb]$ sudo crackmapexec smb 172.16.5.5 -u forend -p Klmcargo2 --shares

SMB         172.16.5.5      445    ACADEMY-EA-DC01  [\*] Windows 10.0 Build 17763 x64 (name:ACADEMY-EA-DC01) (domain:INLANEFREIGHT.LOCAL) (signing:True) (SMBv1:False)

SMB         172.16.5.5      445    ACADEMY-EA-DC01  [+] INLANEFREIGHT.LOCAL\forend:Klmcargo2

SMB         172.16.5.5      445    ACADEMY-EA-DC01  [+] Enumerated shares

SMB         172.16.5.5      445    ACADEMY-EA-DC01  Share           Permissions     Remark

SMB         172.16.5.5      445    ACADEMY-EA-DC01  -----           -----------     ------

SMB         172.16.5.5      445    ACADEMY-EA-DC01  ADMIN$                          Remote AdminSMB         172.16.5.5      445    ACADEMY-EA-DC01  C$                              Default shareSMB         172.16.5.5      445    ACADEMY-EA-DC01  Department Shares READ

SMB         172.16.5.5      445    ACADEMY-EA-DC01  IPC$            READ            Remote IPCSMB         172.16.5.5      445    ACADEMY-EA-DC01  NETLOGON        READ            Logon server share

SMB         172.16.5.5      445    ACADEMY-EA-DC01  SYSVOL          READ            Logon server share

SMB         172.16.5.5      445    ACADEMY-EA-DC01  User Shares     READ

SMB         172.16.5.5      445    ACADEMY-EA-DC01  ZZZ\_archive     READ

~~~

1. **Spider\_plus**

~~~ bash

Foresty@htb[/htb]$ sudo crackmapexec smb 172.16.5.5 -u forend -p Klmcargo2 -M spider\_plus --share 'Department Shares'

SMB         172.16.5.5      445    ACADEMY-EA-DC01  [\*] Windows 10.0 Build 17763 x64 (name:ACADEMY-EA-DC01) (domain:INLANEFREIGHT.LOCAL) (signing:True) (SMBv1:False)

SMB         172.16.5.5      445    ACADEMY-EA-DC01  [+] INLANEFREIGHT.LOCAL\forend:Klmcargo2

SPIDER\_P... 172.16.5.5      445    ACADEMY-EA-DC01  [\*] Started spidering plus with option:

SPIDER\_P... 172.16.5.5      445    ACADEMY-EA-DC01  [\*]        DIR: ['print$']

SPIDER\_P... 172.16.5.5      445    ACADEMY-EA-DC01  [\*]        EXT: ['ico', 'lnk']

SPIDER\_P... 172.16.5.5      445    ACADEMY-EA-DC01  [\*]       SIZE: 51200

SPIDER\_P... 172.16.5.5      445    ACADEMY-EA-DC01  [\*]     OUTPUT: /tmp/cme\_spider\_plus

~~~


# 2. Initial Enumeration - Initial Recon Of The Domain


**Enumerating Using NMAP**

   `sudo nmap -v -A -iL hosts.txt -oN /home/htb-student/Documents/host-enum`
**Kerbrute - Internal AD Username Enumeration**

   username wordlist : <https://github.com/insidetrust/statistically-likely-usernames>
   > Download Kerbrute, Untuk Compile nya liat di HTB Academy section Initial Enumeration

**Enumerating Users with Kerbrute**

Enumerating User With Kerbrute
~~~ bash
Foresty@htb[/htb]$ kerbrute userenum -d INLANEFREIGHT.LOCAL --dc 172.16.5.5 jsmith.txt -o valid_ad_users
2021/11/17 23:01:46 >  Using KDC(s):
2021/11/17 23:01:46 >   172.16.5.5:88
2021/11/17 23:01:46 >  [+] VALID USERNAME:       jjones@INLANEFREIGHT.LOCAL
2021/11/17 23:01:46 >  [+] VALID USERNAME:       sbrown@INLANEFREIGHT.LOCAL
2021/11/17 23:01:46 >  [+] VALID USERNAME:       tjohnson@INLANEFREIGHT.LOCAL
2021/11/17 23:01:50 >  [+] VALID USERNAME:       evalentin@INLANEFREIGHT.LOCAL

 <SNIP>

2021/11/17 23:01:51 >  [+] VALID USERNAME:       sgage@INLANEFREIGHT.LOCAL
2021/11/17 23:01:51 >  [+] VALID USERNAME:       jshay@INLANEFREIGHT.LOCAL
2021/11/17 23:01:51 >  [+] VALID USERNAME:       jhermann@INLANEFREIGHT.LOCAL
2021/11/17 23:01:51 >  [+] VALID USERNAME:       whouse@INLANEFREIGHT.LOCAL
2021/11/17 23:01:51 >  [+] VALID USERNAME:       emercer@INLANEFREIGHT.LOCAL
2021/11/17 23:01:52 >  [+] VALID USERNAME:       wshepherd@INLANEFREIGHT.LOCAL
2021/11/17 23:01:56 >  Done! Tested 48705 usernames (56 valid) in 9.940 seconds
~~~

Initial Enumeration - Password Spraying



**Using enum4linux**


~~~ bash
Foresty@htb[/htb]$ enum4linux -U 172.16.5.5  | grep "user:" | cut -f2 -d"[" | cut -f1 -d"]"

administrator

guest

krbtgt

lab\_adm

htb-student

avazquez

pfalcon

fanthony

wdillard

lbradford

sgage

asanchez

dbranch

ccruz

njohnson

mholliday

~~~

**Using RpcClient**

~~~ bash

Foresty@htb[/htb]$ rpcclient -U "" -N 172.16.5.5

rpcclient

$> enumdomusers

user:[administrator] rid:[0x1f4]

user:[guest] rid:[0x1f5]

user:[krbtgt] rid:[0x1f6]

user:[lab\_adm] rid:[0x3e9]

user:[htb-student] rid:[0x457]

user:[avazquez] rid:[0x458]

~~~ 

**Using Crackmapexec**

~~~ bash

Foresty@htb[/htb]$ crackmapexec smb 172.16.5.5 --users

SMB         172.16.5.5      445    ACADEMY-EA-DC01  [\*] Windows 10.0 Build 17763 x64 (name:ACADEMY-EA-DC01) (domain:INLANEFREIGHT.LOCAL) (signing:True) (SMBv1:False)

SMB         172.16.5.5      445    ACADEMY-EA-DC01  [+] Enumerated domain user(s)

SMB         172.16.5.5      445    ACADEMY-EA-DC01  INLANEFREIGHT.LOCAL\administrator                  badpwdcount: 0 baddpwdtime: 2022-01-10 13:23:09.463228

SMB         172.16.5.5      445    ACADEMY-EA-DC01  INLANEFREIGHT.LOCAL\guest                          badpwdcount: 0 baddpwdtime: 1600-12-31 19:03:58

SMB         172.16.5.5      445    ACADEMY-EA-DC01  INLANEFREIGHT.LOCAL\lab\_adm                        badpwdcount: 0 baddpwdtime: 2021-12-21 14:10:56.859064

SMB         172.16.5.5      445    ACADEMY-EA-DC01  INLANEFREIGHT.LOCAL\krbtgt                         badpwdcount: 0 baddpwdtime: 1600-12-31 19:03:58

SMB         172.16.5.5      445    ACADEMY-EA-DC01  INLANEFREIGHT.LOCAL\htb-student                    badpwdcount: 0 baddpwdtime: 2022-02-22 14:48:26.653366

SMB         172.16.5.5      445    ACADEMY-EA-DC01  INLANEFREIGHT.LOCAL\avazquez

~~~

**Gathering Users with LDAP Anonymous**

~~~ bash

Foresty@htb[/htb]$ ldapsearch -h 172.16.5.5 -x -b "DC=INLANEFREIGHT,DC=LOCAL" -s sub "(&(objectclass=user))"  | grep sAMAccountName: | cut -f2 -d" "guest

ACADEMY-EA-DC01$

ACADEMY-EA-MS01$

ACADEMY-EA-WEB01$

htb-student

avazquez

pfalcon

fanthony

wdillard

lbradford

sgage

asanchez

dbranch

~~~ 

**Using windapsearch**

~~~ bash

Foresty@htb[/htb]$ ./windapsearch.py --dc-ip 172.16.5.5 -u "" -U

[+] No username provided. Will try anonymous bind.

[+] Using Domain Controller at: 172.16.5.5

[+] Getting defaultNamingContext from Root DSE

[+]	Found: DC=INLANEFREIGHT,DC=LOCAL

[+] Attempting bind

[+]	...success! Binded as:

[+]	 None

[+] Enumerating all AD users

[+]	Found 2906 users:

cn: Guest

cn: Htb Student

userPrincipalName: htb-student@inlanefreight.local

cn: Annie Vazquez

userPrincipalName: avazquez@inlanefreight.local

cn: Paul Falcon

userPrincipalName: pfalcon@inlanefreight.local

cn: Fae Anthony

userPrincipalName: fanthony@inlanefreight.local

cn: Walter Dillard

userPrincipalName: wdillard@inlanefreight.local

~~~ 

Initial Enumeration - Enumerating Using RPCLIENT



## Using rpcclient

Using rpcclient

~~~ bash

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

Server Role:	ROLE\_DOMAIN\_PDC

Unknown 3:	0x1

~~~ 

## Obtaining the Password Policy using rpcclient

### Obtaining the Password Policy using rpcclient

~~~ bash

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

Server Role:	ROLE\_DOMAIN\_PDC

Unknown 3:	0x1

rpcclient $> getdompwinfomin\_password\_length: 8

password\_properties: 0x00000001

DOMAIN\_PASSWORD\_COMPLEX

~~~

### Initial Enumeration - Enumerating Using enum4linux-ng



**Using enum4linux-ng**

~~~ bash

Foresty@htb[/htb]$ enum4linux -P 172.16.5.5<SNIP>

\==================================================

|    Password Policy Information for 172.16.5.5    |

\==================================================

[+] Attaching to 172.16.5.5 using a NULL share

[+] Trying protocol 139/SMB...

[!] Protocol failed: Cannot request session (Called Name:172.16.5.5)

[+] Trying protocol 445/SMB...

[+] Found domain(s):

[+] INLANEFREIGHT

[+] Builtin

[+] Password Info for Domain: INLANEFREIGHT

[+] Minimum password length: 8

[+] Password history length: 24

[+] Maximum password age: Not Set

[+] Password Complexity Flags: 000001

[+] Domain Refuse Password Change: 0

[+] Domain Password Store Cleartext: 0

[+] Domain Password Lockout Admins: 0

[+] Domain Password No Clear Change: 0

[+] Domain Password No Anon Change: 0

[+] Domain Password Complex: 1

[+] Minimum password age: 1 day 4 minutes

[+] Reset Account Lockout Counter: 30 minutes

[+] Locked Account Duration: 30 minutes

[+] Account Lockout Threshold: 5

[+] Forced Log off Time: Not Set

[+] Retieved partial password policy with rpcclient:

Password Complexity: Enabled

Minimum Password Length: 8

enum4linux complete on Tue Feb 22 17:39:29 2022

~~~

### Initial Enumeration - Enumerating Using ldapsearch



**Using ldapsearch**

~~~ bash

#IPPSEC HTB SAUNA

Foresty@htb[/htb]$ ldapsearch -h 172.16.5.5 -x -b "DC=INLANEFREIGHT,DC=LOCAL" -s sub "\*" | grep -m 1 -B 10

pwdHistoryLengthforceLogoff: -9223372036854775808

lockoutDuration: -18000000000

lockOutObservationWindow: -18000000000

lockoutThreshold: 5

maxPwdAge: -9223372036854775808

minPwdAge: -864000000000

minPwdLength: 8

modifiedCountAtLastProm: 0

nextRid: 1002

pwdProperties: 1

pwdHistoryLength: 24

~~~

