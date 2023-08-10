# Guide from generating SSH-key to deploying spring bot App on Tmux Server.

## Generating SSH Key on Ubuntu

### Intro of SSH

SSH is a **S**ecure **Sh**ell network protocol that is used to communicate between two computers over the internet, one is known as the client machine and the other is known as the host machine. Using this, we can access the machine to manage it or transfer some files. For this, we have to install the SSH server on the client as well as on the host machine, and then we can access the client machine from the host machine using the command line interface.

To connect to the authorized client machine, we generate the SSH keys which are unique and can connect to the host machine after entering those unique SSH keys into the client machine. In the next section, we will explore a method by which we can generate the SSH keys on Ubuntu.

### Enable SSH on Ubuntu

#### 1. Install open-ssh server
To install openssh-server run the following command in the terminal. Open terminal by shortcut command `ctrl + alt + T`.
```
sudo apt install openssh-server -y
```
Output:
```
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
The following additional packages will be installed:
  ncurses-term openssh-sftp-server ssh-import-id
Suggested packages:
  molly-guard monkeysphere ssh-askpass
The following NEW packages will be installed:
  ncurses-term openssh-server openssh-sftp-server ssh-import-id
. . .
. . .
. . .
. . .
```

#### 2. Checking Status of open-ssh server
When the installation done, try checking the status of the openssh-server using `systemctl` command:
```
sudo systemctl status ssh
```
Output:
```
● ssh.service - OpenBSD Secure Shell server
     Loaded: loaded (/lib/systemd/system/ssh.service; enabled; vendor preset: enabled)
     Active: active (running) since Thu 2023-08-10 14:49:49 PKT; 4s ago
       Docs: man:sshd(8)
             man:sshd_config(5)
   Main PID: 129612 (sshd)
      Tasks: 1 (limit: 18777)
     Memory: 1.7M
        CPU: 23ms
     CGroup: /system.slice/ssh.service
             └─129612 "sshd: /usr/sbin/sshd -D [listener] 0 of 10-100 startups"
    . . .
    . . .
    . . .
    . . .
```
#### 3. Allow Connection on ssh port
SSH is running now, let's allow the connection on the SSH port using `ufw` command:
```
sudo ufw allow ssh
```
Output:
```
Rules updated
Rules updated (v6)
```
#### 4. Save the changes
To save the changes we have made to ufw, we need to enable and reload the `ufw` using the following command:
```
sudo ufw enable && sudo ufw reload
```
Output:
```
Firewall is active and enabled on system startup
Firewall reloaded
```

And we're done with enabling SSH on ubuntu.

###









