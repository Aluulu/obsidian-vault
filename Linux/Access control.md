In Linux, each file (or object) has three different levels of permission hierarchy, these are:
- Owner
- Group (often referred to as a "group owner")
- Other

The owner of the object can set permissions to whatever they would like, either restrictively, or allowing open access. They also have full permissions to edit the permissions for the group, and everyone else.
A group of users can be given specific permissions to a file if they are a part of the same group, this group is found in `/etc/group`.
Other refers to everyone else not a part of the above two categories. 

You can determine the ownership of a file with the following command:
```Shell
ls -l [file_name]
```

It will output something similar to the following:
`-rw-r--r-- 1 callum admin 9595977728 Aug  2 22:53 rhel-9.2-x86_64-dvd.iso`

- `-` - Represents the type of file. In this case it's a file. If it was a `d`, it would represent a directory
- `rw-` - Refers to the owner's permissions. In this case it's read and write
- `r--` - Refers to the group's permissions. In this example it's only read
- `r--` - Refers to the other's permissions. Again they are given read only permissions

- `1` - This number indicates the number of hard links to the file. In most cases this number will be `1` as it refers to the file itself
- `callum` - The username of the owner
- `admin` - The name of the group
- `9595977728` - The size of the file in bytes
- `Aug 2 22:53` - This is the file's last modification date and time
- `rhel-9.2-x86_64-dvd.iso` - The name of the file

>[!Important]
>Both the kernel and the filesystem may use numbers to track owners (UIDs) and groups (GIDs). You can check `/etc/passwd` to see what numbers are mapped.
>Text names are only used in a human-readable format. Commands must look up and map names to UIDs and GIDs if they wish to display these text names.

# Root (Superuser)

The root account is UNIX's administrative user, similar to the admin account in Windows. The actual name is superuser, although it is typically referred to as root. The defining characteristic of root is that its UID is 0.

Traditionally Linux and UNIX allows a process with the effective UID of 0 to perform any valid operation it wishes as long as it is is a legal action (for example, executing a program that does not have the executable tag).

During log in, root runs the commands required to handle system logins; as in these programs are run with the UID of 0. Once a log in has been successful, the login process and the shell changes from the UID of 0 to the one matching the logged in user. Once it has changed, it can no longer elevate back to a UID of 0. 

## Logging into root

Ideally root should be unable to be logged into for numerous reasons. This is why some distributions such as Ubuntu disable it by default.
When logging into root, you leave no records of what operations were performed. This can be a problem when recovering from past steps, or when an attacker gets root access. Another reason is that if multiple users are using the root account, you are unable to tell what user had access to it and what they were doing.

You can disable root login with the following command:
```Shell
passwd -l root
```

To re-enable the root login (NOT RECOMMENDED), use the following command:
```Shell
sudo passwd -u root
```

### Su (substitute user)

Instead of directly logging into root, the command `su` (substitute user) and `sudo` (superuser do). The command `su` without any arguments will prompt for the root password, then start a root shell. Unlike when directly logging in, `su` creates a log entry that states who became root and when, but does NOT record commands ran. A neat thing you can do is also use `su` to substitute the identity of a user with a user specific problem to troubleshoot. Typically, you must be a member of the group `wheel` to use `su`.

Examples of this command includes:

Prompt the root login and change the shell to root
```Shell
su
```

Prompt the login of another user and change the shell to that user's UID
```Shell
su [user_name]
```
### Sudo (Superuser do)

`sudo`  is a command that takes a command passed to it as the argument, and runs said command as root.
`sudo` checks the file `/etc/sudoers` for a list of users who are authorised to use `sudo` and the command they are allowed to run on each host. Unlike `su`, `sudo` will prompt the user for their own password if it checks they are able to run `sudo` and the appropriate command.

What sets `sudo` apart from logging into root is three factors. The first being the security of auto-timing the user out and requiring them to put their password in again after a 5 minute period. The second being that `sudo` can limit users to certain commands so certain troublesome commands cannot be ran. The third is that it keeps a log of commands. These can be down in the following locations

- Ubuntu - `/var/log/auth.log`
- RHEL - `/var/log/secure`

The main advantages to using sudo for access control are the following:
- Accountability is much improved because of command logging.
- Users can do specific chores without having unlimited root privileges.
- The real root password can be known to only one or two people.
- Using sudo is faster than using su or logging in as root.
- Privileges can be revoked without the need to change the root password.
- A canonical list of all users with root privileges is maintained.
- The chance of a root shell being file unattended is lessened.
- A single file can control access for an entire network.
#### Sudo configuration file (sudoers)

You can edit who can access `sudo` and what commands they can run by editing the sudoers file found at `/etc/sudoers`.

>[!Warning]
>It is important to edit this file with the command `visudo` rather than using a command like `nano` or a text-based GUI program. This is because `visudo` will check for syntax and spelling errors. If this file is not written correctly, `sudo` will not be able to run.

Each permission specification line includes information about
- The users to whom the line applies
- The hosts on which the line should be heeded
- The commands that the speci ed users can run
- The users as whom the commands can be executed

It is possible to define aliases for sets of users and for sets of users as whom commands may be run.
## Other system accounts

Root is not the only system account with elevated privileges, other system accounts exist that are able to modify important system files or important applications. These accounts are typically created by applications that elevated permissions; for example [[Nginx]] creates an account called `www-data` that has the ability to access files within `/var`, in which most users do not have.

Typically, but not always, accounts with UIDs within the following numbers are categorised by their purpose:
- 0-10 - Used by System accounts
- 11-100 - Used by programs that require elevated permissions

It is good practise to replace the encrypted password of these users in `/etc/shadow` and `/etc/passwd` with a star * so that the account cannot be logged into. It is also good practise to set these account's shells to `/bin/false` or `/bin/nologin` to protect against remote login exploits that use alternative login methods such as SSH key files.
# Elevating permissions for files (setuid and setgid)

The kernel and the filesystem work together to allow specific files to run with elevated permissions, typically ran as root. This can be handy for letting developers or administrators for setting up ways for unprivileged users to perform privileged operations. When a program with "setuid" or "setgid" permission is run by the kernel, the process's permissions are temporarily elevated to match those of the program's owner, rather than the user who started the program.

An example of this is user passwords. Typically they are stored in either `/etc/master.passwd` or `/etc/shadow`.  These files should only have read and write privileges for the root account, but a user should be able to edit their own passwords. In this case, a user can temporarily elevate into root to gain access to reset their password, and then it will de-elevate to lock the file from being edited.

>[!Warning]
>Programs that use setuid and setgid, especially to run as root, are security problems. While the commands set up by default are technically more secure, workarounds and security holes have been discovered in the past and will be found again in the future.
>The best way to minimise this risk is to limit how many programs are using setuid and setgid. Also, do not install programs that were not written with these in mind.

If you wish to use setuid and setgid, you can do so with the following commands:

```Shell
# Runs the program with the same permissions as the file owner
chmod u+s [file_name]
```

```Shell
# Runs the program with the same permissions as the file group
chmod g+s [file_name]
```

```Shell
# Runs the program under root
sudo chown root:root [file_name]
```

>[!Important]
>You can disable setuid and setgid execution of individual filesystems by specifying the `nosuid` option to `mount`. ideally this should be used for filesystems that contain user's home directories or less trustworthy administrative domains.

