# Active Directory

## Users & Groups

### Net.exe

Net.exe installed by default on all Windows operating system, Therefore it would be the first go to in domain enumeration.

{% hint style="info" %}
**Note:** \
The `Net.exe` misses subgroups memberships
{% endhint %}

Enumerate domain users.

```batch
net user /domain
```

Enumerate information of specific user such as group membership, password last set or whether if the account is active.

```batch
net user <user> /domain
```

Enumerate domain groups.

```batch
net group /domain
```

Enumerate group members.

```batch
net group <group> /domain
```

### PowerShell and .NET Classes

Basic function which retrieve LDAP Queries using .NET Classes.

The Function uses two important .NET Classes:

* **`System.DirectoryServices.DirectoryEntry`**: This class represents a node or object in an Active Directory hierarchy
* **`System.DirectoryServices.DirectorySearcher`**: This class is used to perform queries against Active Directory.

{% code title="ldap.ps1" %}
```powershell
function LDAPSearch {
    param (
        [string]$LDAPQuery
    )
    $PDC = [System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain().PdcRoleOwner.Name
    $DistinguishedName = ([adsi]'').distinguishedName
    $DirectoryEntry = New-Object System.DirectoryServices.DirectoryEntry("LDAP://$PDC/$DistinguishedName")
    $DirectorySearcher = New-Object System.DirectoryServices.DirectorySearcher($DirectoryEntry, $LDAPQuery)

    return $DirectorySearcher.FindAll()
}
```
{% endcode %}

Enumerate domain users.

```powershell
LDAPSearch -LDAPQuery "(objectClass=user)"
```

Enumerate specific user properties.

{% code overflow="wrap" %}
```powershell
(LDAPSearch -LDAPQuery "(sAMAccountName=<username>)") | ForEach-Object { $_.Properties }
```
{% endcode %}

Enumerate domain groups.

```powershell
LDAPSearch -LDAPQuery "(objectClass=group)"
```

Enumerate specific group properties.

{% code overflow="wrap" %}
```powershell
(LDAPSearch -LDAPQuery "(cn=<group_name>)") | ForEach-Object { $_.Properties }
```
{% endcode %}

Enumerate specific group members.

```powershell
(LDAPSearch -LDAPQuery "(cn=<group_name>)").Properties["member"]
```

Enumerate specific domain user memberships.

```powershell
(LDAPSearch -LDAPQuery "(sAMAccountName=<username>)").Properties["memberOf"]
```

Enumerate specific domain group membership

```powershell
LDAPSearch -LDAPQuery "(memberOf=CN=<group_name>,OU=Groups,DC=domain,DC=com)"
```

## Domain Controller

Retrieve the Domain Controller with the **`PdcRoleOwner`** which indicates its the main DC.

```powershell
[System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain()
```



