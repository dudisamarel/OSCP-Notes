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

# Command Injection

```bash
$(<Command>)
&& <Command>
& <Command>
| <Command>
;ls # URL ENCODE ';'

# A Snippet to know if the injection is running cmd or powershell
(dir 2>&1 *`|echo CMD);&<# rem #>echo PowerShell
```
