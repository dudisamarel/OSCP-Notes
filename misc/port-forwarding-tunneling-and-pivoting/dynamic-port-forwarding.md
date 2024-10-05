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

# Dynamic Port Forwarding

Dynamic port forwarding uses SSH to create a SOCKS proxy, allowing traffic to be routed through a tunnel to access external resources.

## SSH

```bash
ssh -N -D <ssh_client_ip>:<ssh_client_port> <ssh_server_username>@<ssh_server_ip>
```

This creates a SOCKS proxy on specified port , routing traffic through the pivot machine interface.

To use it with `proxychains`, add to `/etc/proxychains4.conf`:

{% code title="/etc/proxychains4.conf" %}
```
socks5 <ip> <port>
```
{% endcode %}

### Remote dynamic forwarding

{% hint style="info" %}
The SSH server is the attacker local machine.
{% endhint %}

```
ssh -N -R <local_port> <ssh_server_username>@<ssh_server_ip>
```
