The Freqtrade webserver is not secure and all traffic will be send unencrypted over the Internet. A possible man in the middle attack or somebody that eavesdrop on your local network can scan your traffic and find out your username and password to your bot server.

In this final section the webserver traffic will be secured with a SSL/TLS certificate and will therefore be safe to use over the internet. However you should always be aware of possible malicious attacks on your bot like DDOS for example. The best security is not to have your bot be reachable from the internet...

**This option is only available when your bot is on a server with a domain name attached to its IP address!**

## Determine your system

Before installing LetsEncrypt, determine the Linux system you use:

```
cat /etc/os-release
```

## Install Certbot packages

The certbot is an script that automatically downloads and installs the security certificates for your webserver.

To install the packages use:

```
sudo apt update

sudo apt upgrade

apt install certbot python3-certbot-nginx
```

## Run the certbot for the first time

```
certbot --nginx -d <YOURDOMAIN>
```

Answer the questions and continue...

The final question is to redirect HTTP traffic to HTTPS. It is obvious that we choose to redirect the traffic to HTTPS.

## Reload your HTTP webpage

Reload your HTTP webpage and see that the webpage redirects to a HTTPS port. You can also see that the lock icon appears before your URL.

## Renew your Certbot manually

The certbot automatically renews the bot's certificate. 

To override this by hand, use the following command:

```
certbot renew
```

You can use the ``--dry-run`` option to test this without actually renewing the certificate.
