---
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# PowerView

**PowerView** is a PowerShell toolset for AD enumeration, useful for penetration testing. It helps gather domain info, user/group details, inspect permissions, and identify network shares. Commonly used in Active Directory enumeration.

## Domain Information

Retrieves details about the current AD domain (name, domain controller, forest info).

```powershell
Get-NetDomain
```

## Users & Groups

Lists all users in the AD domain with usernames and account status.

```powershell
Get-NetUser
```

Displays common name, last password set date, and last logon for each user in the domain.

```powershell
Get-NetUser | select cn,pwdlastset,lastlogon
```

Lists all groups in the AD domain.

```powershell
Get-NetGroup | select cn
```

Retrieves members of a specific group (e.g., Domain Admins).

```powershell
 Get-NetGroup "<group_name>" | select -ExpandProperty member
```

## Computers

Retrieves a list of all computers in the domain.

```powershell
Get-NetComputer
```

Lists domain computers, showing their operating system and DNS hostname.

```powershell
Get-NetComputer | select operatingsystem,dnshostname
```

## Local Admin

Searches for machines where the current user has local administrator access.

```powershell
Find-LocalAdminAccess
```

## SPN

Enumerates users with SPNs (Service Principal Names) registered.

```powershell
Get-NetUser -SPN | select samaccountname,serviceprincipalname
```

## AD Objects permissions

#### Common Object Permissions:

* **`GenericAll`**: Full control and permissions over the object.
* **`GenericWrite`**: Allows modification of specific attributes on the object.
* **`WriteOwner`**: Grants the ability to change the ownership of an object.
* **`WriteDACL`**: Allows modification of the Discretionary Access Control List (ACE's) for the object.
* **`AllExtendedRights`**: Grants extended rights like resetting or changing passwords.
* **`ForceChangePassword`**: Allows forcing a password change for an object.
* **`Self`**: Grants the ability to add oneself to a group or similar actions.

**SecurityIdentifier (SID)**: This is the unique identifier for an AD Object which has the permission on the target object.

Displays the Access Control List (ACL) for a target AD object.

```powershell
Get-ObjectAcl -Identity <target>
```

Filtering the output in order to display the important information.

{% code overflow="wrap" %}
```powershell
Get-ObjectAcl -Identity "<target_object>" | select SecurityIdentifier,ActiveDirectoryRights
```
{% endcode %}

## Shares

Enumerates shared folders across domain computers.

```powershell
Find-DomainShare
```

## MISC

Convert SIDs to Names

```powershell
"<sid>,<sid>,<sid>,..." | Convert-SidToName
```

Lists active sessions on a specific machine.

```powershell
Get-NetSession -ComputerName <computer>
```

## References

{% embed url="https://github.com/PowerShellMafia/PowerSploit/blob/master/Recon/PowerView.ps1" %}
PowerView
{% endembed %}
