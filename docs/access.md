# How to access HPC resources

DTU Sophia is accessed via the [Secure Shell protocol](https://www.ssh.com/ssh/command/) 
and hence a [ssh client](https://en.wikipedia.org/wiki/Comparison_of_SSH_clients)
is needed on the user's laptop from which connection is to be established to DTU Sophia.
This is preferably installed via the laptop operating system's software package manager.


## When outside DTU network

### DTU Employee

Use your laptop's ssh client from a terminal or create a 
[virtual private network](https://en.wikipedia.org/wiki/Virtual_private_network) (VPN)
connection.

| Tool | URL |
| ---- | --- |
| ssh client; e.g. [OpenSSH](https://www.openssh.com/) | ssh.risoe.dk |
| vpn client; e.g. [OpenConnect](https://www.infradead.org/openconnect/) (Linux) or [AnyConnect](https://www.cisco.com/c/en/us/support/docs/smb/routers/cisco-rv-series-small-business-routers/smb5686-install-cisco-anyconnect-secure-mobility-client-on-a-windows.html) (Windows) |  securevpn.ait.dtu.dk |


### DeiC grant holder

Log on the [secureremote](https://secureremote.dtu.dk/) portal and choose **DeiC Sophia Desktop** 
- this will open a Citrix session with Linux CentOS 7.

## Accessing Sophia login node

### Linux

Open a terminal and access Sophia from the commandline;
```
ssh <username>@sophia.dtu.dk
```

### Microsoft Windows

The [MobaXterm](https://mobaxterm.mobatek.net/) Windows application offers a range of features,
including e.g. default graphics forwarding. Use the same command as for 
Linux.
