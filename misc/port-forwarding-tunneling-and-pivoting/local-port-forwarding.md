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

# Local Port Forwarding

Local forwarding allows to forward a port on your local machine to a port on a remote target.

## Socat

A tool for bidirectional data transfer between two endpoints.

Opens a listening TCP port on the local machine on given port and uses **`fork`** to handle multiple connections. Then forwards the incoming traffic to the remote IP on given port

```bash
socat -ddd TCP-LISTEN:<local_port>,fork TCP:<remote_ip>:<remote_port>
```

* **`-ddd`**: Enables debug mode with detailed output

## Netsh

**netsh** (Network Shell) is a command-line utility in Windows that allows for the configuration and management of networking components and settings.

{% hint style="danger" %}
Local Admin is required to use this tool. In order to bypass UAC use it through RDP running batch shell as administrator
{% endhint %}

Adds a rule to forward traffic from `<LISTEN_IP>:<LISTEN_PORT>` to `<TARGET_IP>:<TARGET_PORT>`.

{% code overflow="wrap" %}
```bash
netsh interface portproxy add v4tov4 listenport=<LISTEN_PORT> listenaddress=<LISTEN_IP> connectport=<TARGET_PORT> connectaddress=<TARGET_IP>
```
{% endcode %}

Displays the existing port forwarding rules.

```bash
netsh interface portproxy show all
```

Allows incoming traffic for `<LISTEN_IP>:<LISTEN_PORT>` via TCP.

{% code overflow="wrap" %}
```
netsh advfirewall firewall add rule name="<RULE_NAME>" protocol=TCP dir=in localip=<LISTEN_IP> localport=<LISTEN_PORT> action=allow
```
{% endcode %}

Removes a specified firewall rule.

```bash
netsh advfirewall firewall delete rule name="<RULE_NAME>"
```

Deletes the port forwarding rule for `<LISTEN_IP>:<LISTEN_PORT>`.

{% code overflow="wrap" %}
```bash
netsh interface portproxy del v4tov4 listenport=<LISTEN_PORT> listenaddress=<LISTEN_IP>
```
{% endcode %}

## SSH

<img src="../../.gitbook/assets/file.excalidraw (5).svg" alt="" class="gitbook-drawing">

{% code overflow="wrap" %}
```bash
ssh -N -L <local_ip>:<local_port>:<target_ip>:<target_port> <ssh_server_username>@<ssh_server_ip>
```
{% endcode %}



