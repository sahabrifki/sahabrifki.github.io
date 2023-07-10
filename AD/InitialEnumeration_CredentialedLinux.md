# Initial Enumeration - Credentialed From Linux


## CME - Domain User Enumeration

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

## CME - Domain Group Enumeration

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

~~~

## CME - Logged On Users

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

## Share Enumeration - Domain Controller

~~~ bash

Foresty@htb[/htb]$ sudo crackmapexec smb 172.16.5.5 -u forend -p Klmcargo2 --shares

SMB         172.16.5.5      445    ACADEMY-EA-DC01  [\*] Windows 10.0 Build 17763 x64 (name:ACADEMY-EA-DC01) (domain:INLANEFREIGHT.LOCAL) (signing:True) (SMBv1:False)

SMB         172.16.5.5      445    ACADEMY-EA-DC01  [+] INLANEFREIGHT.LOCAL\forend:Klmcargo2

SMB         172.16.5.5      445    ACADEMY-EA-DC01  [+] Enumerated shares


SMB         172.16.5.5      445    ACADEMY-EA-DC01  ADMIN$  Remote AdminSMB         172.16.5.5      445    ACADEMY-EA-DC01  C$ Default shareSMB         172.16.5.5      445    ACADEMY-EA-DC01  Department Shares READ

SMB         172.16.5.5      445    ACADEMY-EA-DC01  IPC$            READ            Remote IPCSMB         172.16.5.5      445    ACADEMY-EA-DC01  NETLOGON        READ            Logon server share

SMB         172.16.5.5      445    ACADEMY-EA-DC01  SYSVOL          READ            Logon server share

SMB         172.16.5.5      445    ACADEMY-EA-DC01  User Shares     READ

SMB         172.16.5.5      445    ACADEMY-EA-DC01  ZZZ\_archive     READ

~~~

## Spider\_plus

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
