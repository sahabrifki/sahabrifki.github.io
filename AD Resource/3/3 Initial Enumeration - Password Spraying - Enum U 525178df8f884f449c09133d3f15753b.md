# 3. Initial Enumeration - Password Spraying - Enum User to Exploit

Status: Not started

1. **Using enum4linux**
    
    ```python
    Foresty@htb[/htb]$ enum4linux -U 172.16.5.5  | grep "user:" | cut -f2 -d"[" | cut -f1 -d"]"
    
    administrator
    guest
    krbtgt
    lab_adm
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
    ```
    
2. **Using RpcClient**
    
    ```python
    Foresty@htb[/htb]$ rpcclient -U "" -N 172.16.5.5
    rpcclient 
    $> enumdomusers 
    user:[administrator] rid:[0x1f4]
    user:[guest] rid:[0x1f5]
    user:[krbtgt] rid:[0x1f6]
    user:[lab_adm] rid:[0x3e9]
    user:[htb-student] rid:[0x457]
    user:[avazquez] rid:[0x458]
    
    ```
    
3. **Using Crackmapexec**
    
    ```python
    Foresty@htb[/htb]$ crackmapexec smb 172.16.5.5 --users
    SMB         172.16.5.5      445    ACADEMY-EA-DC01  [*] Windows 10.0 Build 17763 x64 (name:ACADEMY-EA-DC01) (domain:INLANEFREIGHT.LOCAL) (signing:True) (SMBv1:False)
    SMB         172.16.5.5      445    ACADEMY-EA-DC01  [+] Enumerated domain user(s)
    SMB         172.16.5.5      445    ACADEMY-EA-DC01  INLANEFREIGHT.LOCAL\administrator                  badpwdcount: 0 baddpwdtime: 2022-01-10 13:23:09.463228
    SMB         172.16.5.5      445    ACADEMY-EA-DC01  INLANEFREIGHT.LOCAL\guest                          badpwdcount: 0 baddpwdtime: 1600-12-31 19:03:58
    SMB         172.16.5.5      445    ACADEMY-EA-DC01  INLANEFREIGHT.LOCAL\lab_adm                        badpwdcount: 0 baddpwdtime: 2021-12-21 14:10:56.859064
    SMB         172.16.5.5      445    ACADEMY-EA-DC01  INLANEFREIGHT.LOCAL\krbtgt                         badpwdcount: 0 baddpwdtime: 1600-12-31 19:03:58
    SMB         172.16.5.5      445    ACADEMY-EA-DC01  INLANEFREIGHT.LOCAL\htb-student                    badpwdcount: 0 baddpwdtime: 2022-02-22 14:48:26.653366
    SMB         172.16.5.5      445    ACADEMY-EA-DC01  INLANEFREIGHT.LOCAL\avazquez
    ```
    
4. **Gathering Users with LDAP Anonymous**
    
    ```python
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
    ```
    
5. **Using windapsearch**
    
    ```python
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
    ```