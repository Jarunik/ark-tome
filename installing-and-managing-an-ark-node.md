# Installing and managing an Ark Node

Installing an Ark node is pretty easy. But you will need to get comfortable with the ubuntu command line in order to keep your node healthy. This guide tries to give you some basics to get started. Don’t forget to dig deeper.

## Getting your own Server
Register on vultr or any other virtual private server (vps) provider.

https://vultr.com

Deploy a server:

1. Any location
2. Ubuntu 16.04
3. 10$/month, 2048 MB Memory (or better)

## Connecting to your own Server

Once the server is deployed you can login with any SSH client. I recommend MobaXterm.

https://mobaxterm.mobatek.net/

You can connect by:

1. Session -> New
2. Choose SSH
3. Enter IP in the Remote Host field
4. Click ok.
 
Now the clicking is done and we can start typing in the command line.

## Installing Ark node

Login with root and the root password from your vps provider.

Change the root password:

```
passwd
```

Create your own user:

```
adduser jarunik
```

Grant your user sudo rights
You want your user to be able to become root/administrator if he wants to. That is the “sudo” right in the linux world.

```
usermod -a -G sudo jarunik
```

Now you will want to login with that new user instead of root.

**If you try to install Ark on root you will get a lot of permission errors and it will not work.**

Download ARKcommander.sh

```
wget https://ark.io/ARKcommander.sh
bash ARKcommander.sh
```

Ark Commander helps you to manage your Ark node and get started with it. The next chapter will explain the basics.

A longer version of this guide can be found here:

https://blog.ark.io/how-to-setup-a-node-for-ark-and-a-basic-cheat-sheet-4f82910719da

## Operating your Ark node

It is recommended to use the ARKcommander.sh unless you really know what you do.

```
bash ARKcommander.sh
```

### Operations with ARKcommander.sh:

1. Install — This will install your complete node and all required modules
2. Reinstall — Deletes your complete node and installs a fresh node.
3. Update — Updates to the new version (if there is a new one)
4. Rebuild Database — Deletes the blockchain and installs a snapshot
5. Set/Reset Secret — Enter your delegate passphrase to forge
6. OS Update — Installs newest ubuntu and module versions
7. Additional Options — Some other stuff you rarely need.

- A. Start — Start up your node
- R. Restart — Restart your node based on new update/config.
- K. Kill — Stop your node
- S. Node Status — Displays status of your delegate (optional)
- L. Log — Rolling log file to check what is happening.
- 0\. Exit — Close the commander.

### Manual operation of the node

Start the node (commander A):

```
cd ark-node
forever start app.js -c config.mainnet.json -g genesisBlock.mainnet.json
```

Stop the node (commander K):

```
forever stopall
```

Check if the node is running:

```
forever list
```

Check the log (commander L):

```
cd ~/ark-node/log/
tail -f ark.log
```

Edit config files (commander 5):

```
cd ark-node
nano config.mainnet.json
```

Install OS updates (commander 6):

```
sudo apt-get update && sudo apt-get upgrade
sudo apt-get dist-upgrade
sudo apt-get autoremove
```

Delete and recreate an empty blockchain:

```
dropdb ark_mainnet
createdb ark_mainnet
```

Delete the ark-node and the blockchain db:

```
rm -rf ark-node
dropdb ark_mainnet
```

### Ubuntu commands (you should know and use)

Check disk space:

```
df -h
```

Switch directories:

```
cd foldername
```

Go to your home directory:

```
cd
```

Go to your ark-node directory

```
cd ~/ark-node
```

List files in your folder:

```
ls
```

Edit a file:

```
nano filename
```

Create a file:

```
touch filename
```

Create a folder:

```
mkdir foldername
```

Delete a file:

```
rm filename
```

Delete a folder:

```
rm -rf foldername
```

Check folder sizes:

```
ls -lh
```

Deactivate sudo password prompt (for user jarunik):

``` 
sudo visudo
#Add the following line at the end of the file
jarunik ALL=(ALL) NOPASSWD: ALL
```

Configure your log-rotate:

```
nano /etc/logrotate.d/ark-logrotate
```

Rebooting (you will have to restart the node):

```
sudo reboot
bash ARKcommander.sh
A
```

Find big files on your server:

```
du -aBM 2>/dev/null | sort -nr | head -n 50 | more
```

## Improving security
You should harden it’s security if you intend to run your server for longer.

The long version can be found here:

https://blog.ark.io/how-to-secure-your-ark-node-541254028616

### Adapt SSH configuration

```
sudo nano /etc/ssh/sshd_config
```

Change to the following settings (choose your own port):

```
Port 55555
LoginGraceTime 60
PermitRootLogin no
X11Forwarding no
MaxStartups 2
```

Restart your ssh server in order that the changed config takes effect:

```
sudo service ssh restart
```

### Install a firewall

```
sudo apt-get install ufw
sudo ufw default deny incoming
sudo ufw allow 55555/tcp
sudo ufw allow 4001/tcp
sudo ufw allow 4002/tcp
sudo ufw enable
```

### Install fail2ban

```
sudo apt-get install fail2ban
sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
sudo nano /etc/fail2ban/jail.local
```

Replace all SSH ports under JAILS with your own port 55555:

```
port = 55555
```

Now restart fail2ban:

```
sudo service fail2ban restart
```

## Register your node on Arkstats
Arkstats shows the current state of the Ark network. It is pretty easy to list your node there.

https://arkstats.net/

You can find the installation guide on github.

https://github.com/dafty/arkstats-reporter

Short installation guide:

```
sudo apt-get install git
git clone -b release https://github.com/dafty/arkstats-reporter.git
cd arkstats-reporter/
bash build.sh
```

## Feedback
If you made it this far please take some time and give me some feedback. I would really love to improve and extend this guide and include some more useful information.

You can find me in all Ark communities as jarunik.
My website with all my Ark information can be found here:
https://arkcoin.net