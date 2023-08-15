<!-- <details> -->
<!-- <summary> -->
<!-- <h2> -->
Jump to a Section
<!-- </h2> -->
<!-- </summary> -->
 - [Generating and Configuring SSH Key on Server](#generating-and-configuring-ssh-key-on-server)
 	- [Intro of SSH](#intro-of-ssh)
  	- [Enable SSH on Ubuntu](#enable-ssh-on-ubuntu)
   		- [1. Install open-ssh server](#1-install-open-ssh-server)
		- [2. Checking Status of open-ssh server](#2-checking-status-of-open-ssh-server)
		- [3. Allow Connection on ssh port](#3-allow-connection-on-ssh-port)
		- [4. Save the changes](#4-save-the-changes)
 	- [Generating SSH Key on Ubuntu step by step:](#generating-ssh-key-on-ubuntu-step-by-step)
		- [1. Checking whether the SSH Key already generated](#1-checking-whether-the-ssh-key-already-generated)
 		- [2. Generate SSH Key](2-generate-ssh-key)
		- [3. Verify key generation](#3-verify-key-generation)
		- [4. Copy SSH keys on host machine]()
<!-- </details> -->
<!-- ## Jump to a Section -->


---

# Guide from generating SSH-key to deploying spring bot App on Tmux Server.

## Generating and Configuring SSH Key on Server

### Intro of SSH

SSH is a **S**ecure **Sh**ell network protocol that is used to communicate between two computers over the internet, one is known as the client machine and the other is known as the host machine. Using this, we can access the machine to manage it or transfer some files. For this, we have to install the SSH server on the client as well as on the host machine, and then we can access the client machine from the host machine using the command line interface.

To connect to the authorized client machine, we generate the SSH keys which are unique and can connect to the host machine after entering those unique SSH keys into the client machine. In the next section, we will explore a method by which we can generate the SSH keys on Ubuntu.

### Enable SSH on Ubuntu

#### 1. Install open-ssh server
To install openssh-server run the following command in the terminal. Open the terminal by shortcut command `ctrl + alt + T`.
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
When the installation is done, try checking the status of the openssh-server using `systemctl` command:
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

And we're done with enabling SSH on Ubuntu.

---

### Generating SSH Key on Ubuntu step by step:

#### 1. Checking whether the SSH Key already generated
To verify, whether you previously generated any SSH Key, try searching for it using the following command:
```
ls -l ~/.ssh/id_*.pub
```
If the results of the above command are similar to the following output, then jump to [this step](#copy-ssh-key-on-host-machine) :

```
-rw-r--r-- 1 shahid shahid 581 Oct 25  2022 /home/shahid/.ssh/id_rsa.pub
```
Otherwise, for something similar to the following proceed, then keep walking with me.

```
ls: cannot aceess '/home/<username>/.ssh/id_*.pub': No such file or directory
```

#### 2. Generate SSH Key
Considering, we don't have any SSH key generated, we will generate the SSH key on the client machine using the following command:
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
After running the previous command, first, it will ask the user to enter some directory where you want to store the SSH keys, and if we hit the "enter" key without typing anything, then it will store the SSh keys on the default path which is displayed in next line. Secondly, it will ask you to enter a passphrase and re-type it again to confirm the passphrase, the passphrase is used to provide more security. But we'll disable this password. As this causes a security breach as well.

#### 3. Verify key generation
Next, we'll verify the generation of SSh keys by running the following command:
```
ls ~/.ssh/id_*
```
It will show the SSH keys file as follows:
```
/home/shahid/.ssh/id_rsa  /home/shahid/.ssh/id_rsa.pub
```
<!----Send text in *.iid_rsa.pub file for Server registeration --->

#### 4. Copy SSH keys on host machine<a name="copy-ssh-key-on-host-machine"></a>
To copy the SSH keys on the machine to which we want to connect remotely(host). For this, we should know the IP address as well as the username of that machine. For the first time, we'll have the user name "remote" and the IP address is "N.N.N.N". So we'll copy it on the host using the following command:
```
ssh-copy-id remote@N.N.N.N
```
The output will be like this:
```
Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'root@N.N.N.N'"
and check to make sure that only the key(s) you wanted were added.
```
I may ask you to enter the password of the remote(host) machine and enter your password to proceed.

#### 5. Connect with machine
Since the SSH keys have been copied to the host machine. Now we'll try connecting with the machine using the following command:
```
ssh root@N.N.N.N
```
Here, `N` is the number showing your ip address quadrants. So the output is like:

```
Welcome to Ubuntu 22.04.2 LTS (GNU/Linux 5.15.0-72-generic aarch64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Thu Aug 10 10:22:23 AM UTC 2023

  System load:  0.0               Processes:             118
  Usage of /:   6.1% of 37.23GB   Users logged in:       0
  Memory usage: 4%                IPv4 address for eth0: N.N.N.N
  Swap usage:   0%                IPv6 address for eth0: n:n:n:n::n


Expanded Security Maintenance for Applications is not enabled.

0 updates can be applied immediately.

Enable ESM Apps to receive additional future security updates.
See https://ubuntu.com/esm or run: sudo pro status


The list of available updates is more than a week old.
To check for new updates run: sudo apt update

Last login: Thu Aug 10 10:13:39 2023 from 39.42.213.154
root@ubuntu-4gb-fsn1-2:~#
```
We can use simply `exit` command to logout from the host machine and close the connection. And that's it, we're done with generating SSH and connecting with the host machine.

---

### Configuring SSH Key on Ubuntu to disable password-based login:
#### 1. Open ssh config file and apply modification
In order to disable the password-enabled login method, open the `sshd_config` file in the client machine using nano command for text editing:
```
sudo nano /etc/ssh/sshd_config
```
File Area to modify:
```
    . . .
    . . .
    . . .
# Don't read the user's ~/.rhosts and ~/.shosts files
#IgnoreRhosts yes

# To disable tunneled clear text passwords, change to no here!
PasswordAuthentication yes
#PermitEmptyPasswords no

# Change to yes to enable challenge-response passwords (beware issues with
# some PAM modules and threads)
KbdInteractiveAuthentication no

# Kerberos options
#KerberosAuthentication no
    . . .
    . . .
    . . .
```
In the nano text editor file text find the `PasswordAuthentication` line and change its no. After that save and exit from the file.

#### 2. Reload the service of SSH
To reload the service of SSH, use `systemctl` command:
```
sudo systemctl restart ssh
```
By hitting enter, nothing will happen and it will reload the service of SSh.

#### 3. Check status of SSH
We'll use `systemctl` command to check the status of SSH:
```
sudo systemctl status ssh
```
Output:
```
● ssh.service - OpenBSD Secure Shell server
     Loaded: loaded (/lib/systemd/system/ssh.service; enabled; vendor preset: enabled)
     Active: active (running) since Thu 2023-08-10 15:31:54 PKT; 1min 17s ago
       Docs: man:sshd(8)
             man:sshd_config(5)
    Process: 131820 ExecStartPre=/usr/sbin/sshd -t (code=exited, status=0/SUCCESS)
   Main PID: 131823 (sshd)
      Tasks: 1 (limit: 18777)
     Memory: 1.7M
        CPU: 30ms
     CGroup: /system.slice/ssh.service
             └─131823 "sshd: /usr/sbin/sshd -D [listener] 0 of 10-100 startups"

Aug 10 15:31:54 shahid-Latitude-3520 systemd[1]: Starting OpenBSD Secure Shell server...
Aug 10 15:31:54 shahid-Latitude-3520 sshd[131823]: Server listening on 0.0.0.0 port 22.
Aug 10 15:31:54 shahid-Latitude-3520 sshd[131823]: Server listening on :: port 22.
Aug 10 15:31:54 shahid-Latitude-3520 systemd[1]: Started OpenBSD Secure Shell server.
```
As it can be seen that the SSH service is running absolutely fine.

---

## Update Server Packages to the latest:
#### 1. Update Packages
To get all the updates on your server, try running the following two commands one after the other:
```
apt update
```
and
```
apt upgrade
```
Here, we don't need to use `sudo`, since we are a root user currently. We'll create a new Non-root user later on.
After running these two commands, this will upgrade the server with a bulk of logs.

#### 2. Reboot server:
To apply the upgrades effectively, we need to reboot the server using the following command:
```
reboot now
```
This will reboot the server and close your connection with the server. Now the upgrades have been applied to the server.
If the package for `reboot` is missing there, we can also install it using `apt install systemd-sysv` command.

---
### Create and Setup non-root user on Server
Creating a new user with sudo permissions allows you to do administration tasks with elevated privileges only when necessary.
#### 1. Logging to your server
First login to your server using the following command:
```
ssh root@<your-server-ip-address>
```

#### 2. Adding new user
Type the following command after logging in to your server to create a new user:
```
adduser <user-name-you-want>
```
replace the `<user-name-you-want>` with the name of the user you want to create. It can be any name and hit the `enter` button. I'm choosing daud

Output:

```
Adding user `daud' ...
Adding new group `daud' (1000) ...
Adding new user `daud' (1000) with group `daud' ...
Creating home directory `/home/daud' ...
Copying files from `/etc/skel' ...
New password: 
Retype new password: 
passwd: password updated successfully
Changing the user information for daud
Enter the new value, or press ENTER for the default
	Full Name []: Daud Ahmad
	Room Number []: 
	Work Phone []: 
	Home Phone []: 
	Other []: 
Is the information correct? [Y/n]
```
After hitting enter it will ask you to enter some password and re-type it. After that will ask you to enter some information for the user. You can as per your will. You may leave it as default as well by just hitting the `Enter` key.

You can check the saved data using the following command, you need to replace `daud` with the name of user:
```
cat /etc/passwd | grep <daud>
```
#### 3. Add newly created user to the Sudoers
To add the user to the `sudo group` user `usermod` command as follows:
```
usermod -aG sudo <daud>
```
Now, we can verify the groups of the user using following:
```
id <daud>
```
and
```
groups <daud>
```
now the user belongs to the primary and the sudo group.

#### 4. Testing sudo Access
We have created a normal user and added it to the `sudo` group, effectively granting it elevated privileges to run administrative tasks. To test its ability to execute the sudo privileged commands try running the following command to switch to new user:
```
su - <daud>
```
Output:
```
<daud>@ubuntu-4gb-fsn1-2:~$
```
Now, we've switched to the new user. Let's try running some sudo privileged command by prepending `sudo` with the command. For instance, try listing down all the content of directory `/root`.
```
sudo ls -la /root
```
Output:
```
[sudo] password for <daud>: 
total 36
drwx------  5 root root 4096 Aug 10 13:56 .
drwxr-xr-x 18 root root 4096 Aug 10 14:53 ..
-rw-------  1 root root  492 Aug 11 06:32 .bash_history
-rw-r--r--  1 root root 3106 Oct 15  2021 .bashrc
drwx------  2 root root 4096 May 23 12:04 .cache
-rw-r--r--  1 root root    0 May 23 12:06 .cloud-locale-test.skip
drwxr-xr-x  3 root root 4096 Aug 10 13:56 .local
-rw-r--r--  1 root root  161 Jul  9  2019 .profile
drwx------  2 root root 4096 Aug 10 10:14 .ssh
-rw-------  1 root root  881 Aug 10 10:14 .viminfo
```
After entering password for the logged user. we will be able to view above files. Finally, we've created a `sudo` access user who can access `root` privileges by using the `sudo` command prepended.

---

## Installing Packages of our need
### Install Python Latest Version
We're going to install Python's latest version. But we'll first try the following commands to install libraries and dependencies necessary to build Python:
```
sudo apt update
```
```
sudo apt upgrade
```
```
sudo apt install build-essential zlib1g-dev libncurses5-dev libgdbm-dev libnss3-dev libssl-dev libreadline-dev libffi-dev libsqlite3-dev wget libbz2-dev
```
Now, install Python's latest release source code using the `wget` command.
```
wget https://www.python.org/ftp/python/3.11.3/Python-3.11.3.tgz
```
This will install the Python file as _archived_ file on our server. Extract the archive using the following command:
```
tar -xf Python-3.11.3.tgz
```

Now move to the `Python-3.11.3` directory and run the `configure` command. This script performs a number of checks to make sure all of the dependencies are present in our system:
```
cd Python-3.11.3
./configure --enable-optimizations
```
Now Start the build process, we can specify the number of cors with `-j` after checking our system cors with the command `nproc`:
```
make -j 12
```
This will take a bit of time and a lot of logs will be thrown to complete the build process. Now, install the Python binaries using this command:
```
sudo make altinstall
```
Here, `altinstall` command will overwrite the default system `python3` binary. 


There we're, **Python3.11** has been installed. Your can verify it using following:
```
python3.11 --version
```
Output:
```
Python 3.11.3
```

### Install JAVA-17

#### 1. Download and Install Oracle JAVA 17
Use the following command to download Oracle JAVA 17 using `wget`:
```
wget https://download.oracle.com/java/17/latest/jdk-17_linux-x64_bin.deb
```
and then this command to install Java:
```
sudo dpkg -i jdk-17_linux-x64_bin.deb
```
If your installations got broken, then head over to set 2, otherwise skip to step 3.

#### 2. Install Dependencies
Update the system using the following commands:
```
sudo apt-get update
```
```
sudo apt-get clean
```
```
sudo apt-get autoremove
```
```
sudo apt --fix-broken install
```
Then install, JAVA
```
sudo dpkg -i jdk-17_linux-x64_bin.deb
```
Verify installation of JAVA using:
```
java --version
```
Output:
```
java 17.0.6 2023-01-17 LTS
Java(TM) SE Runtime Environment (build 17.0.6+9-LTS-190)
Java HotSpot(TM) 64-Bit Server VM (build 17.0.6+9-LTS-190, mixed mode, sharing)
```

#### Alternative Way of installing JAVA

Following is the alternative way of installing JAVA in one go:
```
sudo apt update && sudo apt upgrade -y;
sudo add-apt-repository ppa:linuxuprising/java -y;
sudo apt update;
sudo apt-get install oracle-java17-installer oracle-java17-set-default;

java -version;
```
#### 3. Set the Java Environment Variables on Ubuntu
```
sudo update-alternatives --config java
```
copy the path till “jdk-17” (i.e. `/usr/lib/jvm/jdk-17/`). Open Environment variable file using `nano`:
```
sudo nano /etc/environment/
```

Go to the Last line in the file and add:
```
JAVA_HOME=”/usr/lib/jvm/jdk-17/”
```

Now Load Environment variable using:
```
source /etc/environment/
```
Check `JAVA_HOME` variable:
```
echo $JAVA_HOME
```
Output:
```
/usr/lib/jvm/java-17-oracle
```

### Install Maven
Get System updates and upgade:
```
sudo apt update;
sudo apt upgrade;
```
Now, install `Maven`:
```
sudo apt install maven -y
```
And, we're done with installing Maven:
```
mvn --version
```
Output:
```
Apache Maven 3.6.3
Maven home: /usr/share/maven
Java version: 17.0.6, vendor: Oracle Corporation, runtime: /usr/lib/jvm/java-17-oracle
Default locale: en_US, platform encoding: UTF-8
OS name: "linux", version: "5.15.0-78-generic", arch: "aarch64", family: "unix"
```

---

### Getting started with `tmux`

To start using tmux, type `tmux` on your terminal. This command launches a tmux server, creates a default session (number 0) with a single window, and attaches to it.
```
tmux
```
You can detach from your `tmux` session by pressing `Ctrl+B` then `D`. Tmux operates using a series of keybindings (keyboard shortcuts) triggered by pressing the "prefix" combination. By default, the prefix is `Ctrl+B`. After that, press `D` to detach from the current session.
```
[detached (from session 0)]
```

You're no longer attached to the session, but your long-running command executes safely in the background. You can list active tmux sessions with tmux ls:
```
tmux ls
```

#### Basic `tmux` keybindings
Tmux provides several keybindings to execute commands quickly in a tmux session. Here are some of the most useful ones.

First, create a new tmux session if you're not already in one. You can name your session by passing the parameter `-s {name}` to the `tmux new` command when creating a new session:

```
tmux new -s Session1
```

- `Ctrl+B D` — Detach from the current session.
- `Ctrl+B %` — Split the window into two panes horizontally.
- `Ctrl+B "` — Split the window into two panes vertically.
- `Ctrl+B Arrow Key` (Left, Right, Up, Down) — Move between panes.
- `Ctrl+B X` — Close pane.
- `Ctrl+B C` — Create a new window.
- `Ctrl+B N` or `P` — Move to the next or previous window.
- `Ctrl+B 0 (1,2...)` — Move to a specific window by number.
- `Ctrl+B :` — Enter the command line to type commands. Tab completion is available.
- `Ctrl+B ?` — View all keybindings. Press `Q` to exit.
- `Ctrl+B W` — Open a panel to navigate across windows in multiple sessions.
- `Ctrl+B ,` — Rename a panel window
- `Ctrl+B [` — Switch to scroll view, Press `Q` to exit scrolling.












----
