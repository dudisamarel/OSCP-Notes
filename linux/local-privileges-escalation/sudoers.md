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

# Sudoers

it's possible to restrict a user's `sudo` permissions to specific commands or binaries. This is done by configuring the `/etc/sudoers` file, where certain users can be allowed to run only a defined set of commands with `sudo`.

**Listing Sudo Capabilities**

To view the sudo commands available to the current user, run:

```bash
sudo -l
```

This will list all the commands and binaries the user is permitted to execute with `sudo`, along with any password restrictions.

```bash
User <username> may run the following commands on hostname:
    (ALL) NOPASSWD: /usr/bin/apt
    (ALL) ALL
```

* **`(ALL)`**: Means the user can run commands as any user (e.g., root).
* **`NOPASSWD`**: Indicates no password is required for specific commands listed.

If the binary is known and can be exploited to gain root privileges, check on **`GTFOBins`** under sudo headlines:

{% embed url="https://gtfobins.github.io/" %}
GTFOBins
{% endembed %}
