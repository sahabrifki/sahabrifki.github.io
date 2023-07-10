# Credentialed Enumeration - From Windows


**ActiveDirectory PowerShell Module**

[https://powersploit.readthedocs.io/en/latest/Recon/](https://powersploit.readthedocs.io/en/latest/Recon/)

### Powerview
**Finding Users With SPN Set**
    
    ```
    PS C:\htb> Get-DomainUser -SPN -Properties samaccountname,ServicePrincipalName
    
    serviceprincipalname                          samaccountname
    --------------------                          --------------
    adfsconnect/azure01.inlanefreight.local       adfs
    backupjob/veam001.inlanefreight.local         backupagent
    d0wngrade/kerberoast.inlanefreight.local      d0wngrade
    kadmin/changepw                               krbtgt
    MSSQLSvc/DEV-PRE-SQL.inlanefreight.local:1433 sqldev
    MSSQLSvc/SPSJDB.inlanefreight.local:1433      sqlprod
    MSSQLSvc/SQL-CL01-01inlanefreight.local:49351 sqlqa
    sts/inlanefreight.local                       solarwindsmonitor
    testspn/kerberoast.inlanefreight.local        testspn
    testspn2/kerberoast.inlanefreight.local       testspn2
    ```
    

### **SharpView**

1. Finding Users With SPN Set
    
    ```
    PS C:\htb> .\SharpView.exe Get-DomainUser -Help
    
    Get_DomainUser -Identity <String[]> -DistinguishedName <String[]> -SamAccountName <String[]> -Name <String[]> -MemberDistinguishedName <String[]> -MemberName <String[]> -SPN <Boolean> -AdminCount <Boolean> -AllowDelegation <Boolean> -DisallowDelegation <Boolean> -TrustedToAuth <Boolean> -PreauthNotRequired <Boolean> -KerberosPreauthNotRequired <Boolean> -NoPreauth <Boolean> -Domain <String> -LDAPFilter <String> -Filter <String> -Properties <String[]> -SearchBase <String> -ADSPath <String> -Server <String> -DomainController <String> -SearchScope <SearchScope> -ResultPageSize <Int32> -ServerTimeLimit <Nullable`1> -SecurityMasks <Nullable`1> -Tombstone <Boolean> -FindOne <Boolean> -ReturnOne <Boolean> -Credential <NetworkCredential> -Raw <Boolean> -UACFilter <UACEnum>
    ```
    
2. Identify User With SPN Set
    
    ```powershell
    PS C:\htb> .\SharpView.exe Get-DomainUser -Identity forend
    
    [Get-DomainSearcher] search base: LDAP://ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL/DC=INLANEFREIGHT,DC=LOCAL
    [Get-DomainUser] filter string: (&(samAccountType=805306368)(|(samAccountName=forend)))
    objectsid                      : {S-1-5-21-3842939050-3880317879-2865463114-5614}
    samaccounttype                 : USER_OBJECT
    objectguid                     : 53264142-082a-4cb8-8714-8158b4974f3b
    useraccountcontrol             : NORMAL_ACCOUNT
    accountexpires                 : 12/31/1600 4:00:00 PM
    lastlogon                      : 4/18/2022 1:01:21 PM
    lastlogontimestamp             : 4/9/2022 1:33:21 PM
    pwdlastset                     : 2/28/2022 12:03:45 PM
    lastlogoff                     : 12/31/1600 4:00:00 PM
    badPasswordTime                : 4/5/2022 7:09:07 AM
    name                           : forend
    distinguishedname              : CN=forend,OU=IT Admins,OU=IT,OU=HQ-NYC,OU=Employees,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL
    whencreated                    : 2/28/2022 8:03:45 PM
    whenchanged                    : 4/9/2022 8:33:21 PM
    samaccountname                 : forend
    memberof                       : {CN=VPN Users,OU=Security Groups,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL, CN=Shared Calendar Read,OU=Security Groups,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL, CN=Printer Access,OU=Security Groups,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL, CN=File Share H Drive,OU=Security Groups,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL, CN=File Share G Drive,OU=Security Groups,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL}
    cn                             : {forend}
    objectclass                    : {top, person, organizationalPerson, user}
    badpwdcount                    : 0
    countrycode                    : 0
    usnchanged                     : 3259288
    logoncount                     : 26618
    primarygroupid                 : 513
    objectcategory                 : CN=Person,CN=Schema,CN=Configuration,DC=INLANEFREIGHT,DC=LOCAL
    dscorepropagationdata          : {3/24/2022 3:58:07 PM, 3/24/2022 3:57:44 PM, 3/24/2022 3:52:58 PM, 3/24/2022 3:49:31 PM, 7/14/1601 10:36:49 PM}
    usncreated                     : 3054181
    instancetype                   : 4
    codepage                       : 0
    ```
    
    **PowerView Cheatsheet**
    
    ```bash

    #Enumerasi Domain User Information
    PS C:\htb> Get-DomainUser -Identity mmorgan -Domain inlanefreight.local | Select-Object -Property name,samaccountname,description,memberof,whencreated,pwdlastset,lastlogontimestamp,accountexpires,admincount,userprincipalname,serviceprincipalname,useraccountcontrol
    
    name                 : Matthew Morgan
    samaccountname       : mmorgan
    description          :
    memberof             : {CN=VPN Users,OU=Security Groups,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL, CN=Shared Calendar
                           Read,OU=Security Groups,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL, CN=Printer Access,OU=Security
                           Groups,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL, CN=File Share H Drive,OU=Security
                           Groups,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL...}
    whencreated          : 10/27/2021 5:37:06 PM
    pwdlastset           : 11/18/2021 10:02:57 AM
    lastlogontimestamp   : 2/27/2022 6:34:25 PM
    accountexpires       : NEVER
    admincount           : 1
    userprincipalname    : mmorgan@inlanefreight.local
    serviceprincipalname :
    mail                 :
    useraccountcontrol   : NORMAL_ACCOUNT, DONT_EXPIRE_PASSWORD, DONT_REQ_PREAUTH
    
    ```

   detailed Powerview Function And Parameter Here : [All PowerView Cheatsheet](./PowerView.md)
