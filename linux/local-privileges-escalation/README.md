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

# Local Privileges Escalation

## Basic Enumeration

{% content-ref url="../local-enumeration.md" %}
[local-enumeration.md](../local-enumeration.md)
{% endcontent-ref %}

## Attack Vectors

### Scheduled tasks

{% content-ref url="scheduled-tasks.md" %}
[scheduled-tasks.md](scheduled-tasks.md)
{% endcontent-ref %}

### Password Authentication&#x20;

The `/etc/passwd` file can contain password hashes directly instead of an `x`, indicating that the password hash is stored in `/etc/shadow`. If `/etc/passwd` is writable, it allows the creation of arbitrary users with root privileges.

{% content-ref url="password-authentication.md" %}
[password-authentication.md](password-authentication.md)
{% endcontent-ref %}

### Monitor Process

It possible that the administrative user used command line with sensitive information exposed.

In this situation monitoring the process can reveal this sensitive information

[#processes](./#processes "mention")

### SetUID Binaries and Capabilities

{% content-ref url="setuid-binaries-and-capabilities.md" %}
[setuid-binaries-and-capabilities.md](setuid-binaries-and-capabilities.md)
{% endcontent-ref %}

### Sudoers

it's possible to restrict a user's `sudo` permissions to specific commands or binaries. This is done by configuring the `/etc/sudoers` file, where certain users can be allowed to run only a defined set of commands with `sudo`.

{% content-ref url="sudoers.md" %}
[sudoers.md](sudoers.md)
{% endcontent-ref %}

### Kernel Exploits

{% content-ref url="kernel-exploits.md" %}
[kernel-exploits.md](kernel-exploits.md)
{% endcontent-ref %}

## References

{% embed url="https://www.cyberciti.biz/faq/understanding-etcpasswd-file-format/" %}
`/etc/passwd` format blog
{% endembed %}

{% embed url="https://blog.g0tmi1k.com/2011/08/basic-linux-privilege-escalation/" %}
Basic Linux Privilege Escalation
{% endembed %}

{% embed url="https://www.redhat.com/sysadmin/suid-sgid-sticky-bit" %}
Red Hat - Linux permissions
{% endembed %}

{% embed url="https://github.com/DominicBreuker/pspy" %}
pspy tool
{% endembed %}

{% embed url="https://github.com/peass-ng/PEASS-ng/tree/master/linPEAS" %}
LinPEAS
{% endembed %}

{% embed url="https://github.com/pentestmonkey/unix-privesc-check" %}
