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

* **`-ddd`**: Enables debug mode with detailed output.

## SSH

<img src="../../.gitbook/assets/file.excalidraw (1).svg" alt="" class="gitbook-drawing">

{% code overflow="wrap" %}
```bash
ssh -N -L <local_ip>:<local_port>:<target_ip>:<target_port> <ssh_server_username>@<ssh_server_ip>
```
{% endcode %}
