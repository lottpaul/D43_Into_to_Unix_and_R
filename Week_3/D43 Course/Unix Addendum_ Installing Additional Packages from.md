Over the 56 year history of Unix, many tools have been developed for specific tasks.  However, it would be impossible to have them all installed by default.  So, typically your base Unix contains tools such as `less`, `wc`, `grep`, and other tools. If you need to install other programs, they are often available from a repository.

---

# Windows WSL2 - Installing Tools from Repository
Ubuntu and other Debian variants use a tool called `apt` to interact with a repository.  It allows you to search, update, upgrade, and install programs from the repository.

For different linux distributions, you'll see package managers such as `yum`, `dnf`, or `zypper`

---

# MacOS - Installing Tools from Repository
MacOS provided a very limited set of tools to be downloaded by default.  However, developers have created  **Homebrew** to act as a repository tool.  **Homebrew** acts similar to `apt` for linux

To install **Homebrew**:
```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

To access Homebrew, we use the `brew` command.

---

## Finding a Package
Let's see if we can find the package called `tree`.   The option  `search` works for the `apt` or `brew`

```
$ apt search tree

$ brew search tree

```


## Installing a Package

To install `tree`.   If you get warned that you don't have permissions to install, you may need to type `sudo` prior to the command.  `sudo` stands for `super user do` - it means execute this command with administrative permissions.  This give the command the permissions it needs to write the installed files to the system directories.

```
# For Windows WSL2 Ubuntu
$ sudo apt install tree


#For MacOS
$ brew install tree

```


# Finding the Packages you want
You'll notice that when you search for a package, you may get back a few different options.   Google is a good place to determine which one is what you want.  


**Homebrew** also has some additional pages:  https://formulae.brew.sh/formula/
