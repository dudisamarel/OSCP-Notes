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

# DNS Enumeration

DNS enumeration involves gathering information about a domain's DNS records to map out its structure. Tools like `dig`, `nslookup`, or `dnsenum` can be used to discover subdomains, mail servers, and other DNS entries.

## DNS Entries

| Entry     | Description                                                                                                  |
| --------- | ------------------------------------------------------------------------------------------------------------ |
| **NS**    | **Nameserver record** - contains the name of the authoritative servers hosting the DNS records for a domain. |
| **A**     | **A record** - contains the IPv4 address of a hostname.                                                      |
| **AAAA**  | **Quad A record** - contains the IPv6 of a hostname.                                                         |
| **MX**    | **Mail Exchange record** - contains the name of the email management servers.                                |
| **PTR**   | **Pointer record** - contains the zones for reverse search using IP address.                                 |
| **CNAME** | **Canonical Name record** - contains aliases for other **A** records.                                        |
| **TXT**   | **Text record** - contains any aribary data.                                                                 |



## Enumeration

Retrieve records using `host`:&#x20;

```bash
host -t <record_type> <domain_name>
```

Retrieve DNS records using `nslookup` (useful for live of the land in Windows):

```powershell
nslookup -type=<record_type> <Domain_name> <ns_server>
```

Subdomain brute force:

```bash
for ip in $(cat <wordlist>); do host $ip.<domain_name>; done | grep -v "not found"
```

Brute forcing records

```bash
dnsrecon -d <domain_name> -D <wordlist> -t brt
```

Useful scans to automate the process:

```bash
dnsrecon -d megacorpone.com -t std
dnsenum <domain_name>
```

## References

{% embed url="https://github.com/darkoperator/dnsrecon" %}
dnsrecon
{% endembed %}

{% embed url="https://github.com/SparrowOchon/dnsenum2" %}
dnsenum
{% endembed %}
