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

### **Plink**

**`plink.exe`** is a command-line SSH client from the **PuTTY** suite. It's used for automating SSH tasks like connecting to remote servers, running commands, and port forwarding, without the need for a graphical interface. It's often used in scripts or for non-interactive SSH sessions.

{% code overflow="wrap" %}
```
C:\Windows\Temp\plink.exe -ssh -l <ssh_server_username> -pw <ssh_server_password> -R 127.0.0.1:<ssh_server_listening_port>:127.0.0.1:<target_port> <ssh_server_ip>
```
{% endcode %}
