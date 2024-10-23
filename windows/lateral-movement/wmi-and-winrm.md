# WMI and WinRM

## WMI

[Windows Management Instrumentation](https://learn.microsoft.com/en-us/windows/win32/wmisdk/wmi-start-page) (WMI), is an object-oriented feature  for task automation.\
&#x20;It communicates through RPC over port 135 and capable of creating processes.

WMI is capable of creating processes via the _Create_ method from the _Win32\_Process_ class.

### Remote process

To create a process on the remote target using WM it is **required member of the Administrators local group**.

Create a process using `wmic`:

{% code overflow="wrap" %}
```batch
wmic /node:<target> /user:<username> /password:<password> process call create "<command>"
```
{% endcode %}

Create a process using PowerShell:

```powershell
$username = '<username>';
$password = '<password>';
$secureString = ConvertTo-SecureString $password -AsPlaintext -Force;
$credential = New-Object System.Management.Automation.PSCredential $username, $secureString;
$options = New-CimSessionOption -Protocol DCOM
$session = New-Cimsession -ComputerName <target> -Credential $credential -SessionOption $Options 
$command = '<command>';
Invoke-CimMethod -CimSession $Session -ClassName Win32_Process -MethodName Create -Arguments @{CommandLine =$Command};
```

## WinRM

[Windows Remote Management](https://learn.microsoft.com/en-us/windows/win32/winrm/portal) is Microsoft's implementation of the [WS-Management](https://en.wikipedia.org/wiki/WS-Management) protocol, used for remote host management. It exchanges XML-based messages over HTTP and HTTPS for secure communication.

### WinRS

**Windows Remote Shell** is a command-line tool used for executing commands remotely on a Windows machine using the WinRM protocol. It allows **administrators** to manage remote systems securely, similar to SSH in Linux.

#### Usage:

```bash
winrs -r:<target> -u:<username> -p:<password> <command>
```

## PS-Remoting

**PowerShell Remoting** allows for the remote execution of PowerShell commands and scripts on other Windows machines using the WinRM protocol. It supports interactive sessions.

The following requirements must be met to use PS-Remoting:

* PS-Remoting must be **enabled** on the target machine.
* The user must belong to either the **Administrators** group or the **Remote Management Users** group.

#### Usage:

```powershell
$username = '<username>';
$password = '<password>';
$secureString = ConvertTo-SecureString $password -AsPlaintext -Force;
$credential = New-Object System.Management.Automation.PSCredential $username, $secureString;
PSSession -ComputerName <target> -Credential $credential
```

## References

{% embed url="https://en.wikipedia.org/wiki/WS-Management" %}
WS-Management protocol
{% endembed %}

{% embed url="https://learn.microsoft.com/en-us/windows/win32/winrm/portal" %}
WinRM
{% endembed %}

{% embed url="https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/winrs" %}
winrs
{% endembed %}

{% embed url="https://learn.microsoft.com/en-us/windows/win32/wmisdk/wmi-start-page" %}
WMI
{% endembed %}
