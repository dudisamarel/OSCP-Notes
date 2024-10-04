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

# SetUID Binaries and Capabilities

A **SetUID binary** is an executable that runs with the permissions of its owner, typically **root**, regardless of the user executing it. This allows regular users to perform privileged tasks. However, misconfigured SetUID binaries can be exploited to escalate privileges.

Find SetUID Binaries.

```bash
find / -perm /4000 2>/dev/null
```

**Capabilities** allow more granular control over privileges. For example, the **`cap_setuid`** capability enables a binary to change user IDs without full root privileges, limiting security risks compared to SetUID.

Find SetUID Capabilities

```bash
/usr/sbin/getcap -r / 2>/dev/null | grep cap_setuid
```

Exploit SetUID binaries using **`GTFOBins`**:

**`GTFOBins`** is a  list of Unix binaries that can be used to bypass local security restrictions in misconfigured systems.

{% embed url="https://gtfobins.github.io/" %}
GTFOBins
{% endembed %}
