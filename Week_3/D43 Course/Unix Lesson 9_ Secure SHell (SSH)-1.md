Secure shell (SSH) is a communications protocol which allows you to connect to remote computer's terminal shell. Commands passed over SSH will be executed on the remote computer.  
SSH functions under a server-client model. The Server runs an SSH Daemon process and requires the client to authenticate using username and credentials (password, key).

## Simple SSH

The SSH command takes a username and address to a remote computer. The computer address can be an Domain Name, such as "UCDavis.edu" or an IP address, such as 54.190.129.17

**Usage:** `ssh username@computerAddress`

If this is the first time you've connected to the remote server, you'll recieve an Authenticity statement. This is provided to users, so that they can verify that the computer their connecting to is the one that they want to connect to.

## Credentials for HIVE:
SSH to the HIVE uses your same email and password that you use for all of UCDavis logins


### **Example**

```
$ ssh <username>@hive.hpc.ucdavis.edu

The authenticity of host 'hive.hpc.ucdavis.edu (xxx.xxx.xxx.xxx)' can't be established.
ED25519 key fingerprint is SHA256:QhAr7IN3F0ZgP5qgKl/LaMhDPSUOy6JX1+N7R+mdzkY.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'xxx.xxx.xxx.xxx' (ED25519) to the list of known hosts.

<username@hive's password:

#remote server cursor
<username>@login1 ~ $ 
```

You'll typically see a change to the command prompt to denote that any command that you issue will be executed on the remote computer.

The remote server doesn't have access to your local machine's storage, so the filesystem on the remote server is unique.

If you need to transfer files to the remote server, you can use applications such as: `scp` or `rsync` ; unless you have a SSH server process running on your machine - you'll need to run these from your computer and not from the remote server command prompt.

**Tutorial on Rsync**: https://www.digitalocean.com/community/tutorials/how-to-use-rsync-to-sync-local-and-remote-directories

**Tutorial on SCP:** https://www.geeksforgeeks.org/scp-command-in-linux-with-examples/

Additional graphical applications for transferring files via SSH: [WinSCP](https://winscp.net/eng/index.php), [FileZilla](https://filezilla-project.org/), [Cyberduck](https://cyberduck.io/), [Bitvise Client](https://bitvise.com/)