# Token impersonation

Windows identifies users by generating an access token assigned to each user. This token contains information about the user's privileges. When a user runs a process or thread, the primary token is assigned, specifying the permissions for that process. A thread can also have an impersonation token assigned, which provides a different security context; in this case, the process will run based on the impersonation token instead of the primary token.

## Enumeration

Checking if current user has the **`SeImpersonatePrivilege`** :  &#x20;

```batch
whoami /priv
```

## Potato Exploits

The "Potato" tools exploit misconfigurations or vulnerabilities in how Windows handles inter-process communication, specifically with services that trust and elevate tokens such as **NT AUTHORITY\SYSTEM**.

### **Sweet Potato: The All-in-One King**

[**Sweet Potato**](https://github.com/CCob/SweetPotato) is a modern successor to many previous potato exploits. It's designed to work across various versions of Windows, making it versatile.

### **Rogue Potato** (Windows >= 1809 / Server 2019)

[**Rogue Potato**](https://github.com/antonioCoco/RoguePotato) is a privilege escalation exploit designed for **Windows 10 1809 and above**, including **Windows Server 2019**.&#x20;

### **Juicy Potato** (Windows < 1809 / Server < 2019)

[**Juicy Potato**](https://github.com/ohpe/juicy-potato)  is a privilege escalation exploit designed for **Windows 10 prior to 1809** or **Server prior to 2019.1809 and above**, including **Windows Server 2019**.&#x20;

### **Other Potato Variants**

* [Hot Potato](https://jlajara.gitlab.io/Potatoes\_Windows\_Privesc#hotPotato)
* [Rotten Potato](https://jlajara.gitlab.io/Potatoes\_Windows\_Privesc#rottenPotato)
* [Lonely Potato](https://jlajara.gitlab.io/Potatoes\_Windows\_Privesc#lonelyPotato)
* [Generic Potato](https://jlajara.gitlab.io/Potatoes\_Windows\_Privesc#genericPotato)

## References

{% embed url="https://jlajara.gitlab.io/Potatoes_Windows_Privesc" %}
Potato exploits overall blog
{% endembed %}

{% embed url="https://book.hacktricks.xyz/windows-hardening/windows-local-privilege-escalation/roguepotato-and-printspoofer" %}
Potato usage cheatsheet
{% endembed %}
