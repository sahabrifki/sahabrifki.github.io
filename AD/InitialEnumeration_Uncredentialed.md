# Initial Enumeration - Uncredentialed

**Host Scanning Using NMAP**

   `sudo nmap -v -A -iL hosts.txt -oN /home/htb-student/Documents/host-enum`

**Username Enumeration - Using Kerbrute**

   username wordlist : <https://github.com/insidetrust/statistically-likely-usernames>
   > Download Kerbrute, Untuk Compile nya liat di HTB Academy section Initial Enumeration

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




