# 🐲 OSCP Methodology



## Find the foothold

### Host Discovery

The first go to is to search hosts available on the given subnet.

[host-discovery.md](information-gathering/active-reconnaissance/host-discovery.md "mention")



### Port Scanning

After finding the hosts are available , the next step will be to scan the open ports in order to identify target exposed services.&#x20;

[port-scanning.md](information-gathering/active-reconnaissance/port-scanning.md "mention")



### Identify a vulnerable service

There are many possible attack vectors:

#### Web service

* Brute-force directories and files.
* Brute-force subdomains.
* Read source pages and search for comments or libraries\frameworks vulnerable version.
* Leverage [Broken link](broken-reference "mention").

#### File services

[smtp-25.md](information-gathering/active-reconnaissance/smtp-25.md "mention") or [#smb-1](windows/enumeration/external-enumeration.md#smb-1 "mention") can be useful for  phishing attacks.

To generate the phishing use [client-side.md](windows/client-side.md "mention") attacks or generate exe using [#msfvenom](misc/metasploit.md#msfvenom "mention").

#### SSH

When SSH is open try brute-force it - [#hydra](misc/password-attacks.md#hydra "mention").

Search for the SSH private key `.ssh/id_rsa` using pwned machine or web vulnerability like [directory-traversal.md](web-attacks/directory-traversal.md "mention").



## Privilege escalation

### Linux

First enumerate the system - [local-enumeration.md](windows/enumeration/local-enumeration.md "mention")&#x20;

Then exploit any attack vector in order to escalate to root - [local-privileges-escalation](linux/local-privileges-escalation/ "mention")

### Windows

First enumerate the system - [local-privilege-escalation](windows/local-privilege-escalation/ "mention")

Then exploit any attack vector to escalate to admin - [local-privilege-escalation](windows/local-privilege-escalation/ "mention")

### Active directory compromise

1. Enumerate the system - [local-privilege-escalation](windows/local-privilege-escalation/ "mention")
2. Exploit any attack vector to escalate to root - [local-privilege-escalation](windows/local-privilege-escalation/ "mention")
3. Credential Harvesting - [mimikatz-basics.md](windows/mimikatz-basics.md "mention")
4. Move between ad machines - [lateral-movement](windows/lateral-movement/ "mention")
5. Until getting Domain Admin or DC Sync privilege Repeat 1.
6. [dc-sync.md](windows/authentication-attacks/dc-sync.md "mention")
