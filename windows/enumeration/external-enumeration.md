# External Enumeration

External enumeration involves gathering information about Windows systems from outside the network. Techniques like SMB probing, Nmap scanning, and DNS zone transfers help identify active hosts, exposed services, and potential vulnerabilities before gaining internal access.

## Network Discovery

Identify live hosts and gather basic network information to map out the target environment.

### Nmap

Enumerating hosts using ping scan.

```bash
nmap -sn <TARGET_SUBNET>
```

### Zone Transfer

Zone transfers can reveal detailed DNS information, including records for hosts within the domain.

```bash
dig axfr <domain_name>@<name_server>
```

### Smb

Enumerating `smb` hosts:

```
nxc smb <target_subnet>
```

<figure><img src="../../.gitbook/assets/image (11).png" alt=""><figcaption><p>smb - hosts enumeration</p></figcaption></figure>

{% hint style="info" %}
Note: DC smb signing is true by default.
{% endhint %}

## Service and Port Scanning

Enumerate open ports and running services to identify potential entry points.

### Nmap

Scanning specific hosts:

```bash
nmap -Pn -p- -sC -sV -oN <output_file> <target>
```

* `-Pn` don’t do ping scan
* `-p-` scan the 65535 ports instead of the default `nmap` 1000 top ports
* `-sC` run default script for reconnaissance
* `-sV` enumerate the version
* `-oN` write results in normal format

### RPC

Checking for Null Sessions:

```bash
rpcclient -U "" <target>
```

## Active Directory Reconnaissance

Gather detailed information about Active Directory environments, including users, groups, and policies.

### Find DCs

```bash
nslookup -type=srv _kerberos._tcp.dc._msdcs.<domain> <target_ip>
```

<figure><img src="../../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

### Enum4linux

```bash
enum4linux -U <target>
```

### RPC

Connect anonymously

```bash
rpcclient -U "<domain_name>\\" <target> -N
```

Enumerate users:

```bash
rpcclient $> enumdomusers
```

### SMB

```bash
nxc smb <target> -u '' -p '' --shares
nxc smb <target> -u 'a' -p '' --shares
nxc smb <target> -u 'guest' -p '' --shares
```

Enumerate valid users:

{% hint style="info" %}
Note: blank and not blank usernames can make a difference even when login anonymously.
{% endhint %}

```bash
nxc smb <target> -u '' -p '' -
nxc smb <target> -u 'a' -p '' 
nxc smb <target> -u 'guest' -p ''
```

```bash
# Enumerate users
nxc smb <target> -u '' -p '' --users

# Perform RID Bruteforce to get users
nxc smb <target> -u '' -p '' --rid-brute

# Enumerate domain groups
nxc smb <target> -u '' -p '' --groups

# Enumerate available shares
nxc smb <target> -u '' -p '' --shares
smbclient -L //<target> -N

# Get the active sessions
nxc smb <target> -u '' -p '' --sessions

# Get the password policy
nxc smb <target> -u '' -p '' --pass-pol
```

## Users Enumeration

### NXC

```bash
nxc smb <target> -u <userlist_file> -p <pass> --continue-on-success
```

### Kerbrute

```
kerbrute userenum <userlist_file> -d <domain_name> --dc <dc_ip>
```

## References

{% embed url="https://mayfly277.github.io/posts/GOADv2-pwning_part1/" %}
