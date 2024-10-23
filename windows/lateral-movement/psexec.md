# PsExec

[PsExec ](https://learn.microsoft.com/en-us/sysinternals/downloads/psexec)is a command-line tool from Microsoft's [Sysinternals ](https://learn.microsoft.com/en-us/sysinternals/)suite that allows remote execution of processes on other computers and enables interactive sessions.

In order to use PsExec three requisites must be met:

1. The user authenticating to the target machine must be part of the **local Administrators group**.
2. The **ADMIN$** share must be available.
3. File and Printer Sharing has to be turned on.

To execute commands remotely, PsExec performs the following tasks:

1. Writes **`psexesvc.exe`** into the **`C:\Windows`** directory.
2. Creates and spawns a service on the remote host.
3. Runs the requested program/command as a child process of **`psexesvc.exe`**.

Example command:

```bash
./PsExec64.exe -i\\<remote_computer> -u <username> -p <password> cmd.exe
```
