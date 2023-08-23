<details>
<summary>
<h2>
Jump to a Section
</h2>
</summary>

 - [Generating and Configuring SSH Key on Server](#generating-and-configuring-ssh-key-on-server)
 	- [Intro of SSH](#intro-of-ssh)
  	- [Enable SSH on Ubuntu](#enable-ssh-on-ubuntu)
   		- [1. Install open-ssh server](#1-install-open-ssh-server)
		- [2. Checking Status of open-ssh server](#2-checking-status-of-open-ssh-server)
		- [3. Allow Connection on ssh port](#3-allow-connection-on-ssh-port)
		- [4. Save the changes](#4-save-the-changes)
 	- [Generating SSH Key on Ubuntu step by step:](#generating-ssh-key-on-ubuntu-step-by-step)
		- [1. Checking whether the SSH Key already generated](#1-checking-whether-the-ssh-key-already-generated)
 		- [2. Generate SSH Key](#2-generate-ssh-key)
		- [3. Verify key generation](#3-verify-key-generation)
		- [4. Copy SSH keys on host machine](#4-copy-ssh-keys-on-host-machine)
		- [5. Connect with machine](#5-connect-with-machine)
	- [Configuring SSH Key on Ubuntu to disable password-based login:](#configuring-ssh-key-on-ubuntu-to-disable-password-based-login)
		- [1. Open ssh config file and apply modification](#1-open-ssh-config-file-and-apply-modification)
		- [2. Reload the service of SSH](#2-reload-the-service-of-ssh)
		- [3. Check status of SSH](#3-check-status-of-ssh)
	- [Update Server Packages to the latest:](#update-server-packages-to-the-latest)
		- [1. Update Packages](#1-update-packages)
		- [2. Reboot server:](#2-reboot-server)
 - [Create and Setup non-root user on Server](#create-and-setup-non-root-user-on-server)
	- [1. Logging to your server](#1-logging-to-your-server)
	- [2. Adding new user](#2-adding-new-user)
	- [3. Add newly created user to the Sudoers](#3-add-newly-created-user-to-the-sudoers)
	- [4. Testing sudo Access](#4-testing-sudo-access)
 - [Installing Packages of our need](#installing-packages-of-our-need)
	- [Install Python Latest Version](#install-python-latest-version)
	- [Install JAVA-17](#install-java-17)
		- [1. Download and Install Oracle JAVA 17](#1-download-and-install-oracle-java-17)
		- [2. Install Dependencies](#2-install-dependencies)
		- [Alternative Way of installing JAVA](#alternative-way-of-installing-java)
		- [3. Set the Java Environment Variables on Ubuntu](#3-set-the-java-environment-variables-on-ubuntu)
	- [Install Maven](#install-maven)
 - [Getting started with tmux](#getting-started-with-tmux)
	- [Basic tmux keybindings](#basic-tmux-keybindings)
 - [Database Setup](#database-setup)
	- [PostgreSQL Installation](#postgresql-installation)
	- [Start PostgreSQL CommandLine Tool](#start-postgresql-commandline-tool)
	- [Create New Database](#create-new-database)
 - [Running Spring Boot App in Tmux](#running-spring-boot-app-in-tmux)
	- [Does the Database is up](#does-the-database-is-up)
	- [Create a Tmux Session](#create-a-tmux-session)
	- [Running Spring Boot App](#running-spring-boot-app)
 - [Setup Tomcat and Deploying Spring Boot App in Tomcat](#setup-tomcat-and-deploying-spring-boot-app-in-tomcat)
 	- [Setup and installation of tomcat](#setup-and-installation-of-tomcat)
	  	- [1. Update and upgrades](#1-update-and-upgrades)
	   	- [2. Installation and setup of JAVA](#2-installation-and-setup-of-java)
		- [3. Create a tomcat user](#3-create-a-tomcat-user)
		- [4. Download tomcat](#4-download-tomcat)
		- [5. Update permissions of the installation directory](#5-update-permissions-of-the-installation-directory)
		- [6. Creat systemd Unit File](#6-creat-systemd-unit-file)
		- [7. Run the tomcat service](#7-run-the-tomcat-service)
		- [8. Configuring the Tomcat Web Management Interface](#8-configuring-the-tomcat-web-management-interface)
  	- [Amendments in Spring Boot Application to deploy in Tomcat](#amendments-in-spring-boot-application-to-deploy-in-tomcat)
		- [1. Modify @SpringBootApplication Startup class](#1-modify-springbootapplication-startup-class)
		- [2. Adjustments in pom. xml](#2-adjustments-in-pomxml)
		- [3. Changing in SecurityConfig for setting up servletPath](#3-changing-in-securityconfig-for-setting-up-servletpath)
		- [4. Exporting WAR file for tomcat deployment](#4-exporting-war-file-for-tomcat-deployment)
  	- [Deploy Spring Boot Application in Tomcat](#deploy-spring-boot-application-in-tomcat)
		- [1. Login to tomcat Manager App](#1-login-to-tomcat-manager-app)
		- [2. Upload war file and deploy](#2-upload-war-file-and-deploy)

</details>

<!-- ## Jump to a Section -->


---

# Guide from generating SSH-key to deploying spring bot App in Tmux Session and Tomcat server.

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

### Update Server Packages to the latest:
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
## Create and Setup non-root user on Server
Creating a new user with sudo permissions allows you to do administration tasks with elevated privileges only when necessary.
### 1. Logging to your server
First login to your server using the following command:
```
ssh root@<your-server-ip-address>
```

### 2. Adding new user
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
### 3. Add newly created user to the Sudoers
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

### 4. Testing sudo Access
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
sudo nano /etc/environment
```

Go to the Last line in the file and add:
```
JAVA_HOME="/usr/lib/jvm/jdk-17/"
```

Now Load Environment variable using:
```
source /etc/environment
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

## Getting started with `tmux`

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

### Basic `tmux` keybindings
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

## Database Setup
### PostgreSQL Installation
For our project, we need to have a working connection with a `postgresql` database. Here're couple of commands to install postgresql on Ubuntu:
```
sudo apt install wget ca-certificates;
sudo apt update;
sudo apt install postgresql postgresql-contrib;

service postgresql status;

```
For detailed configuration of the `PostgreSQL` database, one can follow along [this article](https://www.cherryservers.com/blog/how-to-install-and-setup-postgresql-server-on-ubuntu-20-04).

### Start PostgreSQL CommandLine Tool
When you install PostgreSQL a default admin user `postgres` is created by the default. You must use it to log-in to your PostgreSQL database for the first time.

A `psql` command-line client tool is used to interact with the database engine. You should invoke it as a `postgres` user to start an interactive session with your local database.

```
sudo -u postgres psql
```

In addition to creating a postgres admin user for you, PostgreSQL installation also creates a default database named “postgres” and connects you to it automatically when you first launch psql.

After first launching psql, you may check the details of your connection by typing `\conninfo` into the interpreter.
```
\conninfo
```
You are now connected to database `postgres` as user `postgres`.

### Create New Database
You are now connected to your database server through psql command line tool with full access rights, so it’s time to create a new database.
```
CREATE DATABASE mydb;
```
After the new `mydb` database is created, connect to it.
```
\c mydb
```
Additionaly, you can list down all the databases and the user roles using commands, respectively:
```
\l
```
```
\du
```
Since the default "postgres" user does not have a password, you should set it yourself.

```
\password postgres
```


## Running Spring Boot App in `Tmux`
### Does the Database is up
Make sure our database is **up**, we can double-check it using the command:
```
service postgresql status
```
If there is some error, you can follow the previous steps. Make sure you have created the `database` specified in the `application.properties` file. You need to run the application first by specifing the value for `spring.jpa.hibernate.ddl-auto` = `create` and then all next time, it would be `update` or `validate`.
### Create a `Tmux` Session
```
tmux new -s <tmux-session-name>
```
After typing above command and hitting `enter`, we'll enter the tmux session.

### Running Spring Boot App
Go to the directory of the Spring boot project, and run the following command:
```
mvn package
```
Then, on successful Build Creation:
```
mvn spring-boot:run
```
Our Spring Boot Application has started running on the IP address of our server. i.e.,
```
<IP-address:8080>
```
In case, **`Maven`** is not installed:
```
./mvnw package
```
then go to the `target/` directory inside the project:
```
cd target/
```
Then, to list down all `jar` files
```
ls *.jar
```
And to run `jar` file:
```
java -jar <[file name].jar>
```
Another way out is running app from project root directory:
```
./mvnw spring-boot:run
```


---

## Setup `Tomcat` and Deploying Spring Boot App in `Tomcat`
### Setup and installation of `tomcat`
#### 1. Update and upgrades
Before starting the installation of Tomcat and other things on your server, do:
```
sudo apt update
sudo apt upgrade
```
#### 2. Installation and setup of JAVA
Previously, we've installed JAVA here, you can follow these steps to install it. Make sure you set up the environment variable for `JAVA_HOME`, you can check it using:
```
echo $JAVA_HOME
```
#### 3. Create a `tomcat` user
For security, you should not use Tomcat without a unique user. This will make the install of Tomcat on Ubuntu easier. Create a new tomcat group that will run the service:
```
sudo groupadd tomcat
```

Now, the next procedure is to create a new tomcat user. Create user members of the Tomcat group with a home directory `opt/tomcat` for running the Tomcat service:
```
sudo useradd -s /bin/false -g tomcat -d /opt/tomcat tomcat
```
#### 4. Download `tomcat`
We need to download the tomcat `tar.gz` file from official website of [Apache Tomcat](https://tomcat.apache.org/download-10.cgi) and copy the link of core _**tar.gz**_ file under the Binary Distributions section. Here, one thing to mention very importantly that **Spring boot 3** requires at least **JDK17** and **Tomcat 10** due to the package renaming from javax to jakarta. We can use `curl` command or `wget` to download the file. Since, we wouldn't need the file after extraction, so we'll go to the `/tmp` directory of the user and download it there.
```
cd /tmp
```
```
curl -O https://dlcdn.apache.org/tomcat/tomcat-10/v10.1.12/bin/apache-tomcat-10.1.12.tar.gz
```
after `curl -O` we can replace the copied url from the _Apache Tomcat_ website.

#### 5. Update permissions of the installation directory
We will install Tomcat to the `/opt/tomcat` directory. Create the directory, then extract the archive to that folder.
```
sudo mkdir /opt/tomcat
```
```
sudo tar xzvf /tmp/apache-tomcat-9.0.*tar.gz -C /opt/tomcat --strip-components=1
```
Change to the directory where you unpacked the Tomcat.
```
cd /opt/tomcat
```

Now, give the Tomcat group ownership over the entire installation directory with the `chgrp` command:
```
sudo chgrp -R tomcat /opt/tomcat
```
Next, you need to give the Tomcat user access to the `conf` directory to view its contents and execute access to the directory itself:
```
sudo chmod -R g+r conf
```

```
sudo chmod g+x conf
```
Make the Tomcat user the owner of the `web apps`, `work`, `temp`, and `logs` directories:

```
sudo chown -R tomcat webapps/ work/ temp/ logs/
```

#### 6. Creat systemd Unit File
We will need to configure **systemd** service file of tomcat to run Tomcat as a service. For that, we need to setup `JAVA_HOME`.
To look up `JAVA_HOME` location run this command
```
sudo update-java-alternatives -l
```
Output would be like:
```
java-1.8.0-openjdk-amd64       1081       /usr/lib/jvm/java-1.8.0-openjdk-amd64
```
Copy the path `/usr/lib/jvm/java-1.8.0-openjdk-amd64` somewhere. You will need this path to set `JAVA_HOME` later. In your system, `JAVA_HOME` can be different, so don’t worry.

You need to configure `tomcat.service`. That resides in `/etc/systemd/system` directory. Run command
```
sudo nano /etc/systemd/system/tomcat.service
```
Most probably, You will get a empty file opening. You need to paste following inside:

```
[Unit]
Description=Tomcat webs servlet container
After=network.target

[Service]
Type=forking

User=tomcat
Group=tomcat

Environment="JAVA_HOME=/usr/lib/jvm/java-17-openjdk-arm64"
Environment="JAVA_OPTS=-Djava.awt.headless=true -Djava.security.egd=file:/dev/./urandom"

Environment="CATALINA_BASE=/opt/tomcat"
Environment="CATALINA_HOME=/opt/tomcat"
Environment="CATALINA_PID=/opt/tomcat/temp/tomcat.pid"
Environment="CATALINA_OPTS=-Xms512M -Xmx1024M -server -XX:+UseParallelGC"

PIDFile=/opt/tomcat/temp/tomcat.pid

UMask=0007
RestartSec=10
Restart=always

ExecStart=/opt/tomcat/bin/startup.sh
ExecStop=/opt/tomcat/bin/shutdown.sh

[Install]
WantedBy=multi-user.target
```
You have to replace the `JAVA_HOME` path with the your systems related. In some cases, you need to append `/jre` after the path of `JAVA_HOME`. You can try it out. Also, if it does not work try removing `Restart=always` from file. After any change in `tomcat.service` file, you need to `reload-demons` and then `restart tomcat`.

For saving this service click `Ctrl+x` then `Shift+y` then Enter.

#### 7. Run the `tomcat` service

Notify the system that you have created a new file by issuing the following command in the command line:
```
sudo systemctl daemon-reload
```

We can directly try running tomcat service, if some problem araise, it can be seen easily.
```
sudo /opt/tomcat/bin/startup.sh run
```
```
sudo /opt/tomcat/bin/shutdown.sh run
```
But, the actual way of running the `tomcat` service is:

```
sudo systemctl start tomcat
```
Or we can try out to restart the service:

```
sudo systemctl restart tomcat
```
To check tomcat status whether it is running or not, run command:

```
sudo systemctl status tomcat
```
Another command is:
```
sudo systemctl status tomcat.service
```
To Enabel service (optional):
```
sudo systemctl enable --now tomcat
```

Successful Output:
```
● tomcat.service - Tomcat webs servlet container
     Loaded: loaded (/etc/systemd/system/tomcat.service; enabled; vendor preset: enabled)
     Active: active (running) since Wed 2023-08-16 17:48:07 UTC; 1h 57min ago
    Process: 151788 ExecStart=/opt/tomcat/bin/startup.sh (code=exited, status=0/SUCCESS)
   Main PID: 151795 (java)
      Tasks: 32 (limit: 4406)
     Memory: 308.9M
        CPU: 19.880s
     CGroup: /system.slice/tomcat.service
             └─151795 /usr/lib/jvm/java-17-openjdk-arm64/bin/java -Djava.util.logging.config.file=/opt/tomcat/conf/logging.properties -Djava.util.logging.manager=org.apache.juli.ClassLoaderLogManager -Djava.awt>

Aug 16 17:48:07 ubuntu-4gb-fsn1-2 systemd[1]: Starting Tomcat webs servlet container...
Aug 16 17:48:07 ubuntu-4gb-fsn1-2 startup.sh[151788]: Tomcat started.
Aug 16 17:48:07 ubuntu-4gb-fsn1-2 systemd[1]: Started Tomcat webs servlet container.
```

#### 8. Configuring the `Tomcat` Web Management Interface
In order to use the manager web app that comes with Tomcat, you must add a login credential to our Tomcat server. We will do this by editing the `tomcat-users.xml` file. You have to add a user who can access the `manager-gui`, `admin-gui` and `manager-script`. To open `tomcat-users.xml` run command

```
sudo nano /opt/tomcat/conf/tomcat-users.xml
```

When the `xml file` is opened you need to add the following line:
```
<role rolename="admin"/>
<role rolename="admin-gui"/>
<role rolename="manager"/>
<role rolename="manager-gui"/>
<role rolename="manager-script"/>

<user username="admin" password="password" roles="admin,manager,manager-gui,admin-gui,manager-script"/>
```
inside the tag:
```
<tomcat-users . . .>
  .....
</tomcat-users>
```
Now save this `xml file` click `Ctrl+x` then `Shift+y` then `Enter`.

By default, newer versions of Tomcat restrict access to the Manager apps to connections coming from the server itself (throw **403** error sometimes). Since you are installing on a remote machine, you will probably want to remove or alter this restriction. To change the IP address restrictions on these, open the appropriate context.xml files. Run the below command to open `context.xml` file of `mnager`.
```
sudo nano /opt/tomcat/webapps/manager/META-INF/context.xml
```
Inside file, you will see somewhere:
```
<Context antiResourceLocking="false" privileged="true" >
  <Valve className="org.apache.catalina.valves.RemoteAddrValve"
         allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" />
</Context>
```
You need to comment out the tag section of `<Valve>`, i.e., `<!--- ` before the starting tag and ` -->` after the closing tag.

```
<Context antiResourceLocking="false" privileged="true" >
  <!--<Valve className="org.apache.catalina.valves.RemoteAddrValve"
         allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" />-->
</Context>
```
Similarly, We need to do same thing for `Host-manager` by opening file:
```
sudo nano /opt/tomcat/webapps/host-manager/META-INF/context.xml
```
Here, do the same thing to comment out the tag section of `<valve>` and then save the file.

After that `restart` the service to take effects:
```
sudo systemctl restart tomcat
```
To access the Web Managment Interface, enter following url in browser:
```
http://<domain-name or IP-address>:8080/
```
and you will see following interface:

![Screenshot from 2023-08-17 01-21-33](https://github.com/DaudAhmad0303/guide-from-generating-ssh-key-to-deploying-spring-bot-app/assets/73556387/93df8365-718c-4b4a-b3e7-862835ae74f1)

You can Access **Server Status**, **Manager App**, **Host Manager** by entering the `username` and password set in `tomcat-users.xml` file, previously. We'll need to get in `Manager App` to deploy Spring Boot Application.

---

### Amendments in Spring Boot Application to deploy in Tomcat

#### 1. Modify `@SpringBootApplication` Startup class
The main class annotated with `@SpringBootApplication` needs to be inherited from `SpringBootServletInitializer` class. We also need to _override_ `configure` method. After modification it will be like:
```
@SpringBootApplication
public class MySpringBootApplication extends SpringBootServletInitializer {

	@Override
	protected SpringApplicationBuilder configure(SpringApplicationBuilder application)
	{
		return application.sources(MySpringBootApplication.class);
	}

	public static void main(String[] args) {
		SpringApplication.run(MySpringBootApplication.class, args);
	}
}
```
#### 2. Adjustments in `pom.xml`
Change the project packaging to `war` type:
```
<packaging>war</packaging>
```
Add `Spring-boot-tomcat` dependecy in `<dependencies> ... </dependencies>` section.
```
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-tomcat</artifactId>
	<scope>provided</scope>
</dependency>
```
Modify the final `WAR` file name by using the `<finalName>` tag in `<build> ... </build>` section to avoid including the version numbers. We're creating a `WAR` file with the name `web-services`:
```
<finalName>web-services</finalName>
```
#### 3. Changing in `SecurityConfig` for setting up `servletPath`
Since our `DispatcherServlet` is deployed to a different path, i.e., `/web-services/*`, then instead of Secuirty Config:
```
@Bean
SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
    http
        .authorizeHttpRequests((authorize) -> authorize
            .requestMatchers("/spring-mvc/controller/**")).hasRole("USER")
            .anyRequest().authenticated()
        )
        // ...
    return http.build();
}
```
It should be like following to match the URL so that we separae out the servlet path for requests to be correctly matched:
```
@Bean
MvcRequestMatcher.Builder mvc(HandlerMappingIntrospector introspector) {
	return new MvcRequestMatcher.Builder(introspector).servletPath("/web-services");
}

@Bean
SecurityFilterChain filterChain(HttpSecurity http, MvcRequestMatcher.Builder mvc) throws Exception {
    http
        .authorizeHttpRequests((authorize) -> authorize
            .requestMatchers(mvc.pattern("/controller/**")).hasRole("USER")
            .anyRequest().authenticated()
        )
        // ...
    return http.build();
}
```
#### 4. Exporting `WAR` file for tomcat deployment
Now that, we've made all required changes in our Spring Boot Project, next is to export our project's `war` (Web Archive) file to be deployed on tomcat server. For that, run the following command:
```
mvn clean package
```
This command ensures that the project is built from scratch, by cleaning any previous build artifacts and then packaging the project's code and resources into `war` file. This `war` file specific to be `web-services.war` can be found in project home directory under `target` directory.

---

### Deploy Spring Boot Application in Tomcat
#### 1. Login to tomcat `Manager App`
As discussed above, we can login to the tomcat Manager App using credential we've set in previous steps.
![Screenshot_2023-08-23](https://github.com/DaudAhmad0303/guide-from-generating-ssh-key-to-deploying-spring-bot-app/assets/73556387/13127566-2508-4356-95ca-2e6da6c88df7)

After successfull login into `Manager App`, we would enounter following page:
![image](https://github.com/DaudAhmad0303/guide-from-generating-ssh-key-to-deploying-spring-bot-app/assets/73556387/e1972387-ea8a-46dc-8b6b-909341cec656)

#### 2. Upload `War` file and deploy

Now, scroll down to **`WAR file to deploy`** section on same page select `war` file, we've generated in previous step and click on the deploy button. It will take a bit of seconds to deploy. After successful deploy, our application will be listed down under **`Applications`** section as _**web-services**_. Click on it and your application has been deployed.

In main while, when we upload our file from Tomcat Web Interface, it would be uploaded to the following directory and extracted and if it has no errors it would be deployed:
```
sudo ls -la /opt/tomcat/webapps
```
For the status of our deployed Application, we can check the logs of our applications in following directory logs files:
```
sudo ls -la /opt/tomcat/logs
```
We would mostly look into the `catlina.out` file for our tomcat application logs:
```
sudo cat /opt/tomcat/logs/catalina.out
```
Furthermore, our server time is mostly different. So for checking server time and comparing it with ours, we can use following command:
```
timedatectl
```

----
