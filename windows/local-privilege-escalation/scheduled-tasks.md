---
description: >-
  Windows Task Scheduler can execute automated tasks. This tasks can execute
  binary files and also scripts. Also, The scheduled tasks is running behalf on
  user that created the task.
---

# Scheduled Tasks

## Enumeration

The following command list the tasks information in list format:

```batch
schtasks /query /fo LIST /v
```

The results includes:

* The Author
* Next time to run&#x20;
* The target path of the task (program or script)

Searching non-regular tasks inside a list of full built-in tasks can be frustrating, using `Get-`**`ScheduledTasks`** can be helpful to filter known tasks to find non-regular tasks.

{% code overflow="wrap" %}
```powershell
Get-ScheduledTask | ? { $_.Author -notlike "*Microsoft*" -and $_.TaskPath -notlike "*Microsoft\Windows*" }  | Get-ScheduledTaskInfo
```
{% endcode %}

Retrieve more information about the non-regular task

```batch
schtasks /query /v /fo list /tn "<TaskPath><TaskName>"
```

## Exploit

To exploit this it possible to replace the tasks target file, soon as the task will run again the target file will execute.

First, use `icacls` to check the permission of the target file.\
Then replace it with a malicious one and wait for the next time to run.&#x20;

## References

{% embed url="https://learn.microsoft.com/en-us/powershell/module/scheduledtasks/get-scheduledtask?view=windowsserver2022-ps" %}
Get-ScheduledTask
{% endembed %}

{% embed url="https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/schtasks-query" %}
schtasks query
{% endembed %}
