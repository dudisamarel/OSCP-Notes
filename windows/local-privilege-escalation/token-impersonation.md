# Token impersonation

Windows identifies users by generating an access token assigned to each user. This token contains information about the user's privileges. When a user runs a process or thread, the primary token is assigned, specifying the permissions for that process. A thread can also have an impersonation token assigned, which provides a different security context; in this case, the process will run based on the impersonation token instead of the primary token.

## Enumeration

Checking if current user has the `SeImpersonatePrivilege:`  &#x20;

```batch
whoami /priv
```

## Exploit

Using variants from the Potato family tools:

* [Hot Potato](https://jlajara.gitlab.io/Potatoes\_Windows\_Privesc#hotPotato)
* [Rotten Potato](https://jlajara.gitlab.io/Potatoes\_Windows\_Privesc#rottenPotato)
* [Lonely Potato](https://jlajara.gitlab.io/Potatoes\_Windows\_Privesc#lonelyPotato)
* [Juicy Potato](https://jlajara.gitlab.io/Potatoes\_Windows\_Privesc#juicyPotato)
* [Rogue Potato](https://jlajara.gitlab.io/Potatoes\_Windows\_Privesc#roguePotato)
* [Sweet Potato](https://jlajara.gitlab.io/Potatoes\_Windows\_Privesc#sweetPotato)
* [Generic Potato](https://jlajara.gitlab.io/Potatoes\_Windows\_Privesc#genericPotato)

## References

{% embed url="https://jlajara.gitlab.io/Potatoes_Windows_Privesc" %}
