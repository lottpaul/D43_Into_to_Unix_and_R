---
title: 'Unix Lesson 4: File Permissions'
updated: 2025-04-01 23:56:06Z
created: 2025-03-11 00:05:19Z
latitude: 38.71450640
longitude: -121.46280720
altitude: 0.0000
---

# File Permissions in Linux

Unix has a file permission system to determine who can access, modify, or execute files. This basic security features prevents users from deleting or changing important system files or other peoples files, while allowing them to access their own files or files that are shared with a group.

Linux files and directories have 3 basic permissions

- **READ (r)** - Whether you are allowed to view the file or list a directories files
- **WRITE (w)** - Whether you are allowed to change or delete a file
- **EXECUTE (x)** - Whether you can execute a file or enter a directory

To see a file permissions, we can list the files in long format `ls -alh`

```bash
$ ls -alh
-rwxrwxr--  1 plott plott   19M Aug 14  2024 bedtools-2.31.1.tar.gz
drwxrwxrwx 11 plott plott  4.0K Nov  7  2023 bedtools2
```

The permissions are listed first. You'll notice for:

`bedtools-2.31.1.tar.gz` the string `-rwxrwxr--`  
`bedtools2` the string `drwxrwxrwx`

If the first position is a `d`, then it is a directory.  
If the first position is not a `d`, then it is a file.

There are 3 different groupings of `rwx`, these correspond to the 3 different user groups.

## Permission User-Type Categories

There are 3 different types of User-type categories in Unix

1.  User (u) - This is you
2.  Group (g) - This is you and a specific group
3.  Other (o) - All other Users

## Common Commands

| Command | Description |
| --- | --- |
| chmod \[options\]  &lt;file/directory&gt; | Change Permissions |
| chown \[options\] &lt;file/directory&gt; | Change Ownership  (Extra Credit) |

## Changing Permissions

To change a file or directories permissions, we need to know what User-type and what permission we want to change.

We can use two different syntaxes: Letters or Octals

* * *

### Letter Values

- **READ: r**
- **WRITE: w**
- **EXECUTE x**

### Adding Read Permissions

If we want to allow others to read the contents of a file, we need to add the permission (+).

```bash
$ chmod o+r  Dataset.txt
$ ls -alh Dataset.txt

```

### Removing Read Permissions

To remove read permissions for others, we need to subtract the permission (-)

```bash
$ chmod o-r  Dataset.txt
```

### Adding or Subtracting Multiple Permissions and User types

We can add permissions to multiple User-types at one type.

For example, we may want to give Read, Write, Execute permissions to everyone.

```bash
$ chmod ugo+rwx  Dataset.txt


```

* * *

### Octal Values

Octal values are numerical values that correspond to the Letter value.

- **READ: 4**
- **WRITE: 2**
- **EXECUTE 1**

To set more than one permission, you just add the values together.

| Letter | Octal |
| --- | --- |
| `rwx` | 7   |
| `rw-` | 6   |
| `r-x` | 5   |
| `r--` | 4   |
| `-wx` | 3   |
| `-w-` | 2   |
| `--x` | 1   |
| `---` | 0   |

To define the User-Type, we supply `chmod` with 3 different values that correspond to the User-type: Owner, Group, Others

| Owner | Group | Other | Octal Values |
| --- | --- | --- | --- |
| `rwx` | `rwx` | `rwx` | 777 |
| `rwx` | `r-x` | `r-x` | 755 |
| `rwx` | `r-x` | `r-x` | 755 |
| `rw` | `---` | `---` | 600 |

### Adding Read Permissions

If we want to allow others to read the contents of a file, we need to set the octal value to 4

```bash
$ chmod 774  Dataset.txt
$ ls -alh Dataset.txt

```

### Removing Read Permissions

To remove all permissions from, we need to set the octal value to 0

```bash
$ chmod 770 Dataset.txt
```

### Adding or Subtracting Multiple Permissions and User types

We can add permissions to multiple User-types at one type.

For example, we may want to give Read, Write, Execute permissions to everyone.

```bash
$ chmod 777  Dataset.txt
```

* * *

## Directory - Execute Permissions

Directories must have execute (x/1) values set in order to be able to navigate into that directory.

```
$ chmod 666 D43
$ cd D43
-bash: cd: D43: Permission denied
$ chmod 766 D43
```

# Exercise 1: You keep a list of passwords on your computer in a file called `top_secret.txt`. What permission should the permissions of this file be?

# Exercise 2: If you store `top_secret.txt` in a folder called `private` what permissions should the folder have?

&nbsp;

# Move on to Lesson 5: Viewing File Contents

&nbsp;

If you're working on a Cluster or shared computer system, then you may want to take a look at the following:

# Changing Ownership

When you are collaborating on projects, it's useful to allow someone in your same group to be able to access your files.

\*\*Note: if you're working on a collaborative project - it's typical best practice to keep the project folder outside of your `Home` directory in a shared directory. You `Home` directory is meant to be private to you.

In this example, we have a bioinformatics tool we've downloaded and want to make accessible to others in our group.

To see what groups you are a member of type `groups`

```bash
$ groups 
plott adm dialout cdrom floppy sudo audio dip video plugdev users netdev
```

To give `users` group access to these files, we need to change the `group` ownership to `users`

Notice that the default group is the same as your username. Each user is a member of their own group.

```bash
$ ls -alh
-rwxrwxr--  1 plott plott   19M Aug 14  2024 bedtools-2.31.1.tar.gz
drwxrwxrwx 11 plott plott  4.0K Nov  7  2023 bedtools2
$ chown plott:users bedtools

$ ls -R bedtools

$ chown -R plott:users bedtools   #apply change to all files and directories inside of bedtools directory as well.
```

## Advanced Permissions

We're not going to cover these in depth, I'm just going to provide them as a reference because you'll sometimes see them.

| Letter Flag | Description |
| --- | --- |
| _   | No special permission |
| d   | Directory |
| l   | File or Directory is a symbolic link |
| s   | Setuid/setgui permission - runs file with privilege's of owner or group |
| t   | on directory, only owner can delete or rename files |