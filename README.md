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

---
### Generating SSH Key on Ubuntu step by step:

#### 1. Checking whether the SSH Key already generated
To verify, whether you previously generated any SSH Key, try searching for it using following command:
```
ls -l ~/.ssh/id_*.pub
```
If the results of the above command are similar to the following output, then jump to **this step** :

```
-rw-r--r-- 1 shahid shahid 581 Oct 25  2022 /home/shahid/.ssh/id_rsa.pub
```
Otherwise, for something similar to the following out proceed, then keep walking with me.

```
ls: cannot aceess '/home/<username>/.ssh/id_*.pub': No such file or directory
```

#### 2. Generate SSH Key
Considering, we don't have any SSH key generated, So we will generate the SSH key on the client machine using the following commnd:
```
ssh-keygen
```
Output:
```
Generating public/private rsa key pai.
Enter file in which to save the key(/home/<username>/.ssh/id_rsa):
Created directory '/home<username>/.ssh'.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/<username>/.ssh/id_rsa
Your public key has been saved in /home/<username>/.ssh/id_rsa.pub
The key fingerprint is:
    . . .
    . . .
    . . .
    . . .


```
After running previous command, first it will ask the user to enter some direextory where you want to store the SSH keys, and if we hit "enter" key withoout typing anything, then it will store the SSh keys on the default path which is displayed in next line. Secondly, it will ask you to enter a passphrase and re-type it again to confirm the passphrase, the passphrase is used to provide more security. But we'll disable this password. As this cause a security breach as well.

#### 3. Verify key generation
Next, we'll verify the generation of SSh keys by running following command:
```
ls ~/.ssh/id_*
```
It will show the SSH keys file as follows:
```
/home/shahid/.ssh/id_rsa  /home/shahid/.ssh/id_rsa.pub
```

#### 4. Copy SSH keys on host machine
To copy the SSH keys on the machine to which we want to connect remotely(host). For this, we should know the IP address as well as the username of that machine. For the first time, we'll have the user name "remote" and the ip address is "N.N.N.N". So we'll copy is on host use following command:
```
ssh-copy-id remote@N.N.N.N
```
Output will be like:
```
Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'root@N.N.N.N'"
and check to make sure that only the key(s) you wanted were added.
```
I may ask you enter the password of the remote(host) machine, enter your password to proceed.

#### 5. Connect with machine
Since, the SSH keys has been copied to the host machine. Now we'll ry connecting with the machine using following command:
```
ssh root@N.N.N.N
```
Here, 'N' is the number showing your ip address quadrants. And that's it, we're done with generating SSH and connecting with the host machine.














----
