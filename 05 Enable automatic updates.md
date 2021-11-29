## Enable automatic updates

It's important to keep your system up to date with all the latest security updates. To do this in an automatic way, use the snippet below and your system will update automatically.

```
sudo apt update
sudo apt dist-upgrade

sudo apt install unattended-upgrades
sudo dpkg-reconfigure --priority=low unattended-upgrades
```

Choose yes when asked "Automatically download and install stable updates"
