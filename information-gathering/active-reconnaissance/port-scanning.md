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

# Port scanning

**Port scanning** is the process of probing a host for open ports to determine which services are running. Tools like `nmap` or `masscan` can be used to identify open ports and the services behind them.

## Useful scans

Scanning for open ports using Live of the land (LOTL) technique:

```powershell
1..1024 | % {echo ((New-Object Net.Sockets.TcpClient).Connect("192.168.50.151", $_)) "TCP port $_ is open"} 2>$null
```

Scans using `nmap`:

```bash
sudo nmap -Pn -sS -p- <target> # Scan for open ports
sudo nmap -sU <target> # Eunumrate udp ports
sudo nmap -sC -sV -p <ports> <target> # Enumrate open ports
```

Command options:

`-Pn`: skip host discovery.

`-p-`: scan all ports (not recommended at first).

`-p <ports>`: scan specific ports

`sV`: enumerate for version.

`sC`: run default script for enumeration.

`oN <output_file>`: output results in normal format - **ALWAYS DOCUMENT YOUR SCANS**

`sU`: UDP scan.

`sT`: TCP scan.

`sS`: SYN scan.

