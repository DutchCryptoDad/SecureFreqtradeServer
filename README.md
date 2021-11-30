These steps help you to secure your Freqtrade server on a LINUX system.

!!! WARNING !!!

* It's not reccommended to attach your server directly to the Internet!

* It's also not reccommended to expose your API or UI to the internet (via NAT, port tunneling, VPN or something else)!

* Do not share passwords, keyfiles and other security information with others!

The steps you see are based on a [Debian 11](https://www.linuxvminages.com/images/debian-11/) linux system.
This setup is also used on a Cloud VM and on a Raspberri Pi.

Steps in this security process are:

1. Create a normal user and disable the root user
2. Configure a firewall
3. Configure the SSH server for secure login
4. Configure the time server
5. Enable automatic updates for your server
6. Freqtrade bot security hints
7. Create NGINX for a proxy connection to your bot
8. Create a secure web connection with a self-signed certificate

