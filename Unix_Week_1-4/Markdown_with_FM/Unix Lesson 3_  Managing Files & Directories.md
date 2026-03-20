---
title: 'Unix Lesson 3:  Managing Files & Directories'
updated: 2025-04-01 23:54:02Z
created: 2025-03-10 23:48:30Z
latitude: 38.71450640
longitude: -121.46280720
altitude: 0.0000
---

# Lesson 3 Managing Files & Directories

In a typical day using your computer, you may create a file or directory, copy it, move it to a specific directory, rename it, or delete it. Unix provides commands to complete each of these tasks.

This lesson focuses on managing files and directories by performing all the tasks in Unix.

# Commands to Manage Unix Filesystems

| Command | Description |
| --- | --- |
| mkdir &lt;dir&gt; | Create directory |
| rmdir &lt;dir&gt; | Delete directory |
| rm -r &lt;dir&gt; | Recursively delete file/directories |
| rm -rf &lt;file/dir&gt; | Force delete files or directories |
| touch &lt;file&gt; | Create a file, if doesn't exist |
| rm &lt;file&gt; | Delete file |
| cp file1 file2 | Copy file1 to file2 |
| mv &lt;file&gt; file2 | Rename file or directory |
| mv &lt;file&gt; &lt;dir&gt; | Move file to a directory |

## 3.1 Making Directory(s): `mkdir`

`mkdir <dir>` create a directory in your current working directory. You can define the new directory name

```bash
$ cd
$ pwd
/home/plott
$ mkdir test   #Relative path
$ mkdir /home/plott/test1   #Absolute Path
$ ls -alh
drwxr-xr-x  2 plott plott  4.0K Mar 28 17:05 test
drwxr-xr-x  2 plott plott  4.0K Mar 28 17:05 test1
```

If the directory already exists, you'll get an error message.

```bash
$ mkdir test1
mkdir: cannot create directory ‘test1’: File exists
```

`mkdir` by default only creates a single directory in the tree at a time. If try to create a sub-directory in an existing folder, it works. However, if you try to create a sub-directory in a directory that hasn't been created yet.

You'll also receive an error.

```bash
$ mkdir test2/test3
mkdir: cannot create directory ‘test2/test3’: No such file or directory
```

This error message tells you that it can't create the `test3` folder because there isn't a `test2` folder.

`mkdir` has an option `-p` that will create any missing parent directory if it doesn't exist, so the previous command with the `-p` option will create `test2` folder and then the `test3 folder`

```bash
$ mkdir -p test2/test3
$ ls -R    # -R option recursively lists all subfolders and files
.:
test2

./test2:
test3
```

# Exercise 1: Create the following folder structure on your computer using `mkdir`

```bash
.
└── D43_Unix_R_Class
    ├── Books
    │   ├── R
    │   └── Unix
    ├── Cheatsheets
    ├── R
    │   └── Lessons
    │       ├── Lesson_1
    │       ├── Lesson_2
    │       └── Lesson_3
    └── Unix
        └── Lessons
            ├── Lesson_1
            ├── Lesson_2
            ├── Lesson_3
            └── Lesson_4
```

Hint: D43_Unix_R_Class folder contains 4 folder: Books, Cheatsheets, R, and Unix

## 3.2 Deleting Directories

There are a couple commands to delete folders in Unix.

### Safe Method

`rmdir` is a simple command that will delete a single empty directory

To delete `test2` that we created in the previous section, we can use `rmdir` and the absolute/relative path to the folder. This will only delete a single empty folder. If the directory isn't empty, it'll return an error message

```bash
# rmdir test2
rmdir: failed to remove 'test2': Directory not empty
```

`test2` folder contains a directory `test3` so we receive an error message about `test2` not being empty. In order to delete `test2`, we need to first delete `test3`

```
$ rmdir test2/test3
$ rmdir test2
```

### Less Safe Method

`rm` contains an option `-r` to delete files/directories recursively. This can be dangerous because it'll destroy everything in those directories.

**Note: Use this with caution because you can easily delete all your files and folders**

```bash
$ mkdir -p test2/test3
$ ls
$ rm -r test2
$ ls 
```

* * *

## 3.3 Creating Files

You can create a new empty file using `touch` command. This creates an empty file

```bash
$ touch Test.txt
$ touch Readme.md
$ touch MyData.csv
$ ls
MyData.csv  Readme.md  Test.txt
$ ls -alh
total 8.0K
drwxr-xr-x  2 plott plott 4.0K Mar 28 17:47 .
drwxr-x--- 24 plott plott 4.0K Mar 28 17:45 ..
-rw-r--r--  1 plott plott    0 Mar 28 17:47 MyData.csv
-rw-r--r--  1 plott plott    0 Mar 28 17:47 Readme.md
-rw-r--r--  1 plott plott    0 Mar 28 17:47 Test.txt
```

# Exercise 2: Create 3 files: doc1.txt, data1.csv, and deleteme

## 3.4 Removing/Deleting Files

`rm` is used to remove files

```bash
$ rm Test.txt
$ ls
MyData.csv  Readme.md
```

# Exercise 3: Delete the file named deleteme

## 3.5 Copying Files

`cp` is used to copy the *Source File* to *Target File*. We need to supply 2 arguments:

1.  The file you want to copy: *Source File*
2.  The filename or path you want to copy the file to: *Target File/Path*

```bash
$ cp Readme.md Test1.txt
$ ls
MyData.csv  Readme.md  Test1.txt

# Copy to Directory - same name
$ mkdir Test3
$ cp Readme.md Test3
$ ls -R
.
├── MyData.csv
├── Readme.md
├── Test2.txt
└── Test3
    └── Readme.md

# Copy to Directory - different name
$ cp Readme.md Test3/Different.txt
$ ls -R

.
├── MyData.csv
├── Readme.md
├── Test2.txt
└── Test3
    ├── Different.txt
    └── Readme.md
```

**Caution**: When copying files, if the target file already exists. It will automatically be overwritten.

# Exercise 4: Create a Folder called `data` and copy `data1.csv` into `data` folder. Then delete the original copy.

## 3.6 Moving or Renaming Files/Directories

Unix uses the same command to move or rename files/directories. The `mv` command takes a *Source* and *Target*.

```bash
$ mv Readme.md Instructions.txt
$ ls

$ mv Instructions.txt Test3/

$ mv Test3 Documents
$ ls -R

```

# Exercise 5: `data` isn't to informative. Rename the folder `data` to `colon_cancer_dataset` and then rename the file `data1.csv` file to `incidence_report_2025.csv`

# Using Wildcards

Unix wildcards allow you to work with multiple files or directories using pattern matching. They are especially useful when used with commands like ls, rm, cp, or mv.

| Wildcard Character | Description |
| --- | --- |
| `*` | Matches zero or more characters |
| `?` | Matches exactly one character |
| `[...]` | Matches any one character inside the brackets |

```bash
$ ls -R *.txt

$ ls -R *.*

$ ls -R d*

$ ls -R *.[tc]sv

$ cp *.csv Test3/

$ rm *.*

```

# Exercise 6: Do steps b-e using wildcards

a. Create files apple.txt banana.txt cherry.txt data1.csv data2.csv dataA.csv  
b. List all files with .txt suffix  
c. List files that start with `data` followed by a number and end with `.csv`  
d. List files that start with an `a` or `b`  
e. Delete all the `.csv` files

# Move onto Unix Lesson 4: Managing Files and Directories

* * *

# Assignment 1: Create the following file and directory structure using the commands you've learned in the first 3 lessons. Deadline: Sunday April 6th 11:59PM

```
~/.  #Your Home Directory or other starting folder
└── D43_Unix_R_Class
  ├── Books
  │   ├── R
  │   └── Unix
  ├── Cheatsheets
  │   └── My_Notes.md
  ├── Questions
  │   └── CommandPromptIssue.txt
  ├── R
  │   └── Lessons
  │       ├── Lesson_1
  │       ├── Lesson_2
  │       └── Lesson_3
  └── Unix
      └── Lessons
          ├── Lesson_1
          │   └── Exercises_1-10.txt
          ├── Lesson_2
          │   └── Exercises_1-4.txt
          ├── Lesson_3
          │   └── Exercises_1-6.txt
          └── Lesson_4 

```

Provide the history of commands you used and the output from `ls -R` which shows each of the folders shown here and the files in those folders.