## Enable automatic updates

It's important to keep your system up to date with all the latest security updates. To do this in an automatic way, use the snippet below and your system will update automatically.

```
sudo apt update
sudo apt dist-upgrade

sudo apt install unattended-upgrades
```
The following three lines fixes any error that prevents some systems from proceeding with the configuration
```
sudo export LC_ALL=C.UTF-8
sudo update-locale
sudo dpkg-reconfigure locales

sudo dpkg-reconfigure --priority=low unattended-upgrades
```

Choose yes when asked "Automatically download and install stable updates"
