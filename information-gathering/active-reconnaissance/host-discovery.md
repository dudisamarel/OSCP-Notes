# Host Discovery

Host discovery involves identifying live hosts on a network. Tools like `ping`, `nmap`, or `masscan` can be used to discover active systems.

Find hosts:

ping sweep using `nmap`:

```bash
nmap -sn <network_subnet>
```

Ping sweep on Windows:

```batch
for /L %i in (1,1,255) do @ping -n 1 -w 200 <network_subnet>.%i > nul && echo <network_subnet>.%i is up.
```

Ping sweep on Linux:

```bash
for i in {1..254}; do ping -c 1 <network_subnet>.$i | grep "bytes from" & done; wait
```
