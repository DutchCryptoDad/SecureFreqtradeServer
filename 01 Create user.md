Securing your server is one of the most important things. Especually if your server is running on a Cloud service provider and not on a private network (at home).

I used a VM image of [Debian 11](https://www.linuxvmimages.com/images/debian-11/) for this demonstration.

## Create new user and disable root user

The first step is to create a new user that u will use to log in on the server.

Log in with the root:

*NOTE:*  
*The root user on my Debian VM simage is called debian.*  
*Your root user can be called root, depending on what distro u use.*  

```
USERNAME: debian
PASSWORD: debian
```

Then create a new user that you will use for future logins:


```
sudo adduser dcd
```

After answerring the questions the user will be made.

Now give the user sudo rights

```
sudo usermod -a -G sudo  dcd
```

### Test user sudo rights

Logout with the root user and login with user: 

Verify that dcd user can execute sudo commands:

```
# This should not work
ls -la /root

# This should work
sudo ls -la /root
```

The first time the user will be asked his password

### Disable root account for security

Login with newly created user.

Type following command to disable root user debian (in my case):

```
sudo usermod -L debian
```

To check if account is succesfully locked:

```
sudo grep -E --color 'debian' cat /etc/shadow
```

A ! should be added before the encrypted password.

Log in with the disabled root account to see if access is denied.

### Re-enable the old root account

To undo the locking type:

```
sudo usermod -U debian
```

And to check:

```
sudo grep -E --color 'debian' cat /etc/shadow
```

