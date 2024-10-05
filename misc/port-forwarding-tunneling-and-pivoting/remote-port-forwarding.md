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

# Remote Port Forwarding

Remote forwarding allows you to forward a port on the SSH server to a port on your local machine. This is helpful when you want to make a service running on your local machine accessible to the remote server.

## SSH

<img src="../../.gitbook/assets/file.excalidraw (4).svg" alt="" class="gitbook-drawing">

{% code overflow="wrap" %}
```bash
ssh -N -R <local_port>:<local_port>:<target_ip>:<target_port> <ssh_server_username>@<ssh_server_ip>
```
{% endcode %}
