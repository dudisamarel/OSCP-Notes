---
description: >-
  Simple Mail Transport Protocol (SMTP) is the standard email protocol on the
  internet.
---

# SMTP - 25

The SMTP server temporarily stores the messages until they can be relayed to another server or to message store that uses the pop3 or IMAP4 access protocol to communicate with the user's email program.

The SMTP protocol supports commands which allows attacker to enumerate email addresses.

Connect to the SMTP Server:

```bash
nc -nv <target> 25 # netcat
telnet <target> 25 # telnet
```

After establishing the connection its possible to enumerate usernames using the VRFY command

```
VRFY <username>
```

Brute force using `hydra`:

```bash
hydra -P <wordlist> <smtp_server_addr> smtp -V
```

## References

{% embed url="https://www.pcmag.com/encyclopedia/term/smtp" %}
