# Pass The Hash

**Pass The Hash** is a laterale movement technique that uses victim's password NTLM hash in order to authenticate to a remote system or services. Therefore it will only work services that use NTLM authentication.

example for passing the hash using impacket:

```
impacket-psexec -hashes <hash> <username>@<target_ip>
```
