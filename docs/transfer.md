# Data transfer

In this section methods for moving data to and from Sophia are described.

## Using sshfs

Secure SHell FileSystem (SSHFS) allows to mount remote locations on a local machine.
Any user storage resources reachable with `ssh` can be mounted as a file system. 
Since SSHFS creates a user space file system, administrator privileges are not required.

### sshfs on Linux laptop

Install via your package manager or [build from source](https://github.com/libfuse/sshfs).
Then identify **Linux id for your laptop user and primary group**
```md
$ id $USER
```
which are the `uid` and `gid` numbers in the command's output.

On your laptop, create a mount path owned by your local user, e.g.
```md
sudo mkdir -p /mnt/sophia/$USER
sudo chown $USER.$USER /mnt/sophia/$USER
```
and e.g. mount your Sophia `$HOME` path with
```md
sshfs -o uid=<local user uid>,gid=<local user gid> <dtu user>@sophia.dtu.dk:/home/<dtu user> /mnt/sophia/<local user name>
```

### sshfs on Windows laptop

There are [a number of options](https://nelsonslog.wordpress.com/2017/07/19/windows-sshfs-clients/), e.g.

- Install latest [WinSFP from github](https://github.com/billziss-gh/winfsp/releases)
- Install latest [SSHFS-Win from github](https://github.com/billziss-gh/sshfs-win/releases)
- Open File Explorer, *right*-click on `This PC` and choose `Map network drive`. Choose a drive to mount at and in the *Folder* field enter
```md
\\sshfs\<dtu user>@sophia.dtu.dk
```
to mount your Sophia $HOME directory.

## Using sftp

### FileZilla

This is probably one of the easiest and fastest way to transfer files. FileZilla
allows for recursive file transfer, concurrent file transfers, uses incremental
file transfer and allows to resume interrupted file transfers.

[FileZilla](https://sourceforge.net/projects/filezilla/) is a file transfer client,
which runs under Windows, Linux, Mac/IOS and other systems. It is able to transfer
files via sftp and ssh file transfer protocols. For most Linux distributions
pre-compiled packages are available. The HPC clusters support the ssh file transfer
protocol. The set-up is very simple:

*Commands are shown for the Ubuntu operating system and connecting to Jess.*

1.  Install FileZilla.
+   Start FileZilla and open the site manager under File->Site Manager (or press <CTRL>+s).
+   Create a new site with name Sophia.
+   In the general tab, enter:
    1.  Host: `sophia.dtu.dk`
    +   Protocol: SFTP - SSH File Transfer protocol
    +   Logon Type: Ask for Password
    +   User: `<dtu user>`
+   Click `connect` to test the connection.

A particularly nice feature of FileZilla is *synchronized browsing*, which is useful for
keeping entire directory trees in sync. It also allows editing of remote files from the local computer.
