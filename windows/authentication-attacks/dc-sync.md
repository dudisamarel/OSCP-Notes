# DC Sync

## Introduction

In Active Directory, domain controllers **replicate** data to stay synchronized, using the **DRS Remote Protocol** to request updates like user account changes.&#x20;

**DC Sync attack** exploits this process by impersonating a domain controller and **replicating** sensitive information, such as **password hashes**.

In order to perform DC Sync the following privileges are required:

* `DS-Replication-Get-Changes`
* `DS-Replication-Get-Changes-All`

These privileges often granted to **Domain Admins** or **Enterprise Admins.**

**DC Sync** attack allows the attacker to gain further access and launch attacks like **Golden Tickets**.

## Performing the attack

### Mimikatz

```
lsadump::dcsync /user:<user>
```

### Impacket

{% code overflow="wrap" %}
```bash
impacket-secretsdump <domain>/<username>:"<password>"@<ip_address>
```
{% endcode %}
