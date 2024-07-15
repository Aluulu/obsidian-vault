# Owners

The Linux file system is built upon three owners. These are:
- Owner - The owner of the file. This is typically whoever created the directory or file
- Group - Which group has permissions to access said directory or file
- Other - This is all other users that are not the owner and are not a part of the specified group.

# Permissions

These three owners can have a three different permissions to access files. These are:
- Read – Can view or copy file contents
- Write – Can modify file content
- Execute – Can run the file

For directories, the three permissions provide access to:
- Read – Can list all files and copy the files from directory
- Write – Can add or delete files into directory (needs execute permission as well)
- Execute – Can enter the directory (ie `cd` into it)

# Applying permissions

To change the permissions of the owner, group, and other, you use `chmod` to adjust values based on the permissions needed. These values are based on 3 individual digits. These numbers are adjusted based on the permissions you need them to be, these are:

`r` (read) = 4
`w` (write) = 2
`x` (execute) = 1

Each digit of the permissions number may be a sum of 4, 2, 1, and 0. They can be written out as so:

`0` (0+0+0) – No permission.
`1` (0+0+1) – Only execute permission.
`2` (0+2+0) – Only write permission.
`3` (0+2+1) – Write and execute permissions.
`4` (4+0+0) – Only read permission.
`5` (4+0+1) – Read and execute permission.
`6` (4+2+0) – Read and write permissions.
`7` (4+2+1) – Read, write, and execute permission.

These numbers are given to a file to signafy what permissions each owner of the file has, starting with the owner, the group, then others.

So for example if you wanted to give the owner full permission, the group read and execute permissions, and everyone else no access to the file, you can write it out as so:
`750`

Then, using chmod to apply these permissions, you can apply these permissions to a directory or file like so:

```Shell
sudo chmod 750 /file/path
```

# Groups

Groups are the ideal way to do file access. Instead of giving multiple users individual access to a file, instead you add a user to a group and instead modify just that group's permissions.

## List all groups

To list all groups, use the following command:

```Shell
cat /etc/group
```

## List all groups a user is in

To list all the groups a user is in, use the following command:

```Shell
groups [name]
```

For example to find the groups that admin-docker is apart of, use the following command:

```Shell
groups admin-docker
```

## List all users in a group

To list all users in a group, you can use the following command:

```Shell
getent group [group_name]
```

For example to check all users that are apart of the sudo group, type the following:

```Shell
getent group sudo
```

## Create a group

To create a group, you can use the following command:

```Shell
sudo groupadd [groupn_name]
```

So to make a group called `nginx_user`, use the following command:

```Shell
sudo groupadd nginx_user
```

## Add a user into a group

To add a user into a group, you use the following command:

```Shell
sudo usermod -aG [group_name] [username]
```

For example to add the user callum to the sudo group, use the following command:

```Shell
sudo usermod -aG sudo callum
```
# Changing ownership
## Change file/directory owner

To change the owner of a file, use the following command

```Shell
sudo chown [user] [/file/path]
```

For example, to give the account `root` ownership of the file /etc/nginx, paste the following:

```Shell
sudo chown root /etc/nginx
```

To make it recursively own all files and folders, add an `-R` to the command like so:

```Shell
sudo chown -R root /etc/nginx
```
## Change file/directory group

To change the group owner of the file, use the following command

```Shell
sudo chown -R [:group] [/file/path]
```

For example to make a group called `nginx-user` own `/etc/nginx`, you can use the following:

```Shell
sudo chown :nginx-user /etc/nginx
```

To make it recursively own all files and folders, add an `-R` to the command like so:

```Shell
sudo chown -R :nginx-user /etc/nginx
```

## Change permissions

To change permissions on a file, use the `chmod` command and the numeral values in [[#Applying permissions]]. For ease, these are:
`0` (0+0+0) – No permission.
`1` (0+0+1) – Only execute permission.
`2` (0+2+0) – Only write permission.
`3` (0+2+1) – Write and execute permissions.
`4` (4+0+0) – Only read permission.
`5` (4+0+1) – Read and execute permission.
`6` (4+2+0) – Read and write permissions.
`7` (4+2+1) – Read, write, and execute permission.

Use the following command to change the permissions on a file or directory.

```Shell
sudo chmod 770 /file/path/
```

To change the permissions recursively, add a `-R` to the command like so:

```Shell
sudo chmod -R 770 /file/path/
```

