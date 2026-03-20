---
title: >-
  Unix Lesson 10: Customizing your UNIX Profile:  .profile, .bashrc,
  .bashprofile, or .zshrc
updated: 2025-04-21 23:35:12Z
created: 2025-03-25 00:06:11Z
latitude: 38.71450640
longitude: -121.46280720
altitude: 0.0000
---

| File | Purpose | Execution Time |
| :--- | :--- | :--- |
| .bashrc | Used to set up and configure the Bash shell | Executed every time a new terminal window is opened or a new Bash shell is started |
| .bash_profile | Used to set up environment and configurations when logging in to the system | Executed only when the user logs in to the system |
| .profile | Used to set up environment and configurations when logging in to the system | Executed only when the user logs in to the system |

## Bashrc, Bash_Profile, Profile

The `.profile` file is a configuration file that is used to set up and customize the shell environment when a user logs in to the system. This file is typically located in the user's home directory and is executed only once when the user logs in to the system.

This file is used to set up various environment variables, such as the PATH variable, which determines the directories where the shell looks for executable files, and the PS1 variable, which controls the appearance of the shell prompt. Additionally, it can be used to set up aliases, which are short forms of commonly used commands, and to configure other settings, such as the shell's history settings.

For example, you can use the `.profile` file to set up an environment variable that defines the default language for the system, so that when you type `locale` in the terminal, it will show the default language you set up in the `.profile` file.

The `.profile` file is also commonly used to set up custom functions and scripts that can be used to automate certain tasks or to customize the shell's behavior.

It's worth noting that some systems use the `.bash_profile` or `.bashrc` files instead of `.profile`, but the content and purpose of all three files are the same, and you can use any one of them according to your system's preferences.

## .profile

```bash
if [ -f $HOME/.bashrc ]; then
        source $HOME/.bashrc
fi


```

&nbsp;

# .bashrc

```bash
#### Set Default Permission for File Creation
#Files: 666 - Umask(002) = 664
#Folders: 777 - Umask(002) = 775
umask 002

#### Setup Default Editor
export EDITOR=/bin/nano

#### Setup Default Visual Editor
export VISUAL=/bin/nano

#### Setup your Locale and Languange (see all available locales using `locale -a`)
export LANG=en_US.UTF-8
export LC_CTYPE="en_US.UTF-8"
export LC_NUMERIC="en_US.UTF-8"
export LC_TIME="en_US.UTF-8"
export LC_COLLATE="en_US.UTF-8"
export LC_MONETARY="en_US.UTF-8"
export LC_MESSAGES="en_US.UTF-8"
export LC_PAPER="en_US.UTF-8"
export LC_NAME="en_US.UTF-8"
export LC_ADDRESS="en_US.UTF-8"
export LC_TELEPHONE="en_US.UTF-8"
export LC_MEASUREMENT="en_US.UTF-8"
export LC_IDENTIFICATION="en_US.UTF-8"
export LC_ALL=

#### Setup Terminal coloring ####
export CLICOLOR=1
export LSCOLOR="exfxcxdxbxegedabagacad"

#### Setup Prompt Format
export PS1="\[\033[38m\]\u@\h\[\033[01;33m\] \W \[\033[31m\]\[\033[37m\]$\[\033[00m\] "

#### Setup Command History ####
# avoid duplicates..
export HISTCONTROL=ignoredups:erasedups

# append history entries..
shopt -s histappend

# After each command, save and reload history
export PROMPT_COMMAND="history -a; history -c; history -r; $PROMPT_COMMAND"

export HISTTIMEFORMAT="[%m/%d/%y %T ]"
export HISTSIZE=10000
unset HISTFILESIZE

#### Set default aliases
alias diff='diff --color=auto'
alias ls='ls $LS_OPTIONS'
alias edit='nano'
alias grep="grep --color=auto"
alias ps='ps x'
alias mv='mv -i'
alias wget='wget --no-check-certificate'
alias ssel='less +G '

####  Export PATH
# Earlier directories get precedence
export PATH=$HOME/bin:$HOME/.local/bin:$PATH

####  Set Temporary Directory to use
export TMPDIR=/tmp


```

&nbsp;

References:

[https://www.tutorialspoint.com/difference-between-bashrc-bash-profile-and-profile#:~:text=on your needs.-,The .,log in to your system](https://www.tutorialspoint.com/difference-between-bashrc-bash-profile-and-profile#:~:text=on%20your%20needs.-,The%20.,log%20in%20to%20your%20system).

https://www.tecmint.com/set-system-locales-in-linux/

[For ZSH, please see: https://github.com/ohmyzsh/ohmyzsh/blob/master/templates/zshrc.zsh-template](https://github.com/ohmyzsh/ohmyzsh/blob/master/templates/zshrc.zsh-template)