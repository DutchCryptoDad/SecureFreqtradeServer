BEWARE THAT YOU SHOULD AT LEAST HAVE CONSOLE ACCESS TO YOUR SYSTEM BEFORE EXECUTING THE INSTRUCTIONS BELOW!!!

## Secure SSH

To reach your server in a secure way, you should use SSH.

Most Linux distributions have installed SSH by default.

To harden the SSH server, you could use a SSH key to log on the server. This is more secure than a password alone.

### On local client

To make SSH keypairs and disable password logon, use the next steps.

Make a directory that u can use to keep the secure key files:

```
# create hidden directory
mkdir ~/.ssh 

# only local user can use directory
chmod 700 ~/.ssh
```

Create a key pair with the following command. Follow the instructions.  
It's more secure to use a passphrase with the key so that in case that someone has your key, they still cannot use it.

```
ssh-keygen -b 4096

Generating public/private rsa key pair.
Enter file in which to save the key (/home/bas/.ssh/id_rsa): <YOURFILENAME>
Enter passphrase (empty for no passphrase): <YOURPASSPHRASE>
Enter same passphrase again: <YOURPASSPHRASE>
```
After the keypair is made, you can upload the key to the server with:

```
ssh-copy-id dcd@172.16.65.130

```

Use your password when uploading the keys.

### Setup the SSH server

On the server we have to setup some things for a more secure SSH server

First log on to the server with:

```
ssh 'dcd@172.16.65.130'
```

It can be that your client pc wants to save the key password for you in a keychain.

First create a backup of your SSH server configuration

```
sudo cp /etc/ssh/sshd_config{,.org}
```

Now you van configure the SSH server.
Here we will configure SSH to:

* Not allow root to login
* Use the keyfile
* Use another obscure port

```
sudo nano /etc/ssh/sshd_config
```

Change the following lines

```
Port 1970
AddressFamily inet

PermitRootLogin no

AllowUsers dcd

PasswordAuthentication no 
PermitEmptyPasswords no

UsePAM no

X11Forwarding no
```

```
sudo systemctl restart sshd
```

### Add the port to the firewall

If you have set up a firewall (and I hope you did), then you should add the new SSH port to the rules.


You can check the open ports on your system with:

```
sudo ss -atpu
```

If port 1970 is shown, then the firewall can be changed.


```
sudo ufw status

sudo ufw allow 1970/tcp

sudo ufw status
```

Now you should be able to logon to the system with the following command (p = portnumber):

```
ssh 'dcd@172.16.65.130' -p 1970
```

### Cleanup firewall rules

Because we first added a firewall rule for SSH port 22 and since we changed this port to 1970. This old rule for port 22 is not necessary anymore. We can remove these rules with:

```
sudo ufw status numbered

sudo ufw delete <RULENUMBER port 22>
```
