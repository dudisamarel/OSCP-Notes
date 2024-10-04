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

# Scheduled tasks

After identifying the `cron jobs` running on the target, check the permissions of the job script files.\
If the script is editable, modify it to perform malicious actions, such as establishing a reverse shell or adding a new user. \
Once the cron job executes, the modified script will run with the privileges of the task owner, allowing the attacker to gain unauthorized access.

[#scheduled-tasks](./#scheduled-tasks "mention")
