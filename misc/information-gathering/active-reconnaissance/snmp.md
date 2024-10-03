---
description: >-
  Simple Network Management Protocol (SNMP) A widely used network monitoring and
  control protocol.
---

# SNMP

The SNMP  is based on UDP and stateless. moreover, the SNMP protocol version 1,2 and 2c has no traffic encryption which allows attackers to intercept sensitive information.

In addition, The old SNMP versions have weak authentication methods which are usually configured with the default public and private community strings.&#x20;

In order to rederive information about the target, network administrators often rely on Management Information Bases (MIBs), which define the structure of the data that can be queried via SNMP. \
The Object Identifier (OID) is a key component of MIBs, allowing for the precise identification of specific variables and objects within the network.&#x20;

Windows System Overview and Key Components OIDs:

<table><thead><tr><th width="336">OID</th><th>Dsecription</th></tr></thead><tbody><tr><td>1.3.6.1.2.1.25.1.6.0</td><td>System Processes</td></tr><tr><td>1.3.6.1.2.1.25.4.2.1.2</td><td>Running Programs</td></tr><tr><td>1.3.6.1.2.1.25.4.2.1.4</td><td>Processes Path</td></tr><tr><td>1.3.6.1.2.1.25.2.3.1.4</td><td>Storage Units</td></tr><tr><td>1.3.6.1.2.1.25.6.3.1.2</td><td>Software Name</td></tr><tr><td>1.3.6.1.4.1.77.1.2.25</td><td>User Accounts</td></tr><tr><td>1.3.6.1.2.1.6.13.1.3</td><td>TCP Local Ports</td></tr></tbody></table>

## Enumerate exposed SNMP service:

```bash
sudo nmap -sU -p 161,162,10161,10162 <target>
```

## Brute force the Community string <a href="#community-strings" id="community-strings"></a>

```bash
onesixtyone -c <wordlist> -i <targetlist>
```

Useful wordlist:

{% embed url="https://github.com/danielmiessler/SecLists/blob/master/Discovery/SNMP/common-snmp-community-strings.txt" %}
community string wordlist
{% endembed %}

## Enumerating <a href="#enumerating-snmp" id="enumerating-snmp"></a>

Now, after we found that the SNMP's Community strings, its possible to enumerate senetivce information using `snmpwalk`:

```
snmpwalk -c <community_string> -v1 <target>
snmpwalk -c <community_string> -v1 <target> <OID>
```

`-v`: defines the `snmp` version.

`-c`: defines the community sting.

## References

{% embed url="https://www.pcmag.com/encyclopedia/term/snmp" %}
