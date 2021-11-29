The Freqtrade webserver is not secure and all traffic will be send unencrypted over the Internet. A possible man in the middle attack or somebody that eavesdrop on your local network can scan your traffic and find out your username and password to your bot server.

In this final section the webserver traffic will be secured with a SSL/TLS certificate and will therefore be safe to use over the internet. However you should always be aware of possible malicious attacks on your bot like DDOS for example. The best security is not to have your bot be reachable from the internet...

This section is completely copied [How To Create a Self-Signed SSL Certificate for Nginx in Ubuntu 18.04](from https://www.digitalocean.com/community/tutorials/how-to-create-a-self-signed-ssl-certificate-for-nginx-in-ubuntu-18-04) so all credits goes to the writer of the article.

## Create the SSL certificate

To create a self-signed certificate with a RSA key of 2048 bits long and thats valid for 365 days, use the following command:

```
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/nginx-selfsigned.key -out /etc/ssl/certs/nginx-selfsigned.crt
```

Answer the questions for the certificate and move on to the next step:

## Create strong key for negotiating the secure layer

Before any connection is made, the RSA keys have to be exchanged. This section will create a strong session exchange before any traffic is send. See also [Forward secrecy](https://en.wikipedia.org/wiki/Forward_secrecy).

```
sudo openssl dhparam -out /etc/nginx/dhparam.pem 4096
```

## Create files that are going to be used by nginx

Create a new nginx configuration snippet in  
``/etc/nginx/snippets``

```
sudo nano /etc/nginx/snippets/self-signed.conf
```

Add the SSL certificates to this file:

```
ssl_certificate /etc/ssl/certs/nginx-selfsigned.crt;
ssl_certificate_key /etc/ssl/private/nginx-selfsigned.key;
```

Create another snippet with strong encryption settings:

```
sudo nano /etc/nginx/snippets/ssl-params.conf
```

Copy the complete section below into the file:

```
ssl_protocols TLSv1.2;
ssl_prefer_server_ciphers on;
ssl_dhparam /etc/nginx/dhparam.pem;
ssl_ciphers ECDHE-RSA-AES256-GCM-SHA512:DHE-RSA-AES256-GCM-SHA512:ECDHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-SHA384;
ssl_ecdh_curve secp384r1; # Requires nginx >= 1.1.0
ssl_session_timeout  10m;
ssl_session_cache shared:SSL:10m;
ssl_session_tickets off; # Requires nginx >= 1.5.9
ssl_stapling on; # Requires nginx >= 1.3.7
ssl_stapling_verify on; # Requires nginx => 1.3.7
resolver 8.8.8.8 8.8.4.4 valid=300s;
resolver_timeout 5s;
# Disable strict transport security for now. You can uncomment the following
# line if you understand the implications.
# add_header Strict-Transport-Security "max-age=63072000; includeSubDomains; preload";
add_header X-Frame-Options DENY;
add_header X-Content-Type-Options nosniff;
add_header X-XSS-Protection "1; mode=block";
```

## Configure nginx to use these files for ssl

Go to the nginx directory ``/etc/nginx/conf.d`` where we created the file ``freq_proxy.conf`` earlier in step 6.

Backup the file:

```
sudo cp /etc/nginx/conf.d/freq_proxy.conf /etc/nginx/sites-available/freq_proxy.conf.bak
```


Open the file:

```
sudo nano /etc/nginx/conf.d/freq_proxy.conf
```

Edit the file to mach like this:

```
server {
        listen 443;
        listen [::]:443;
        include snippets/self-signed.conf;
        include snippets/ssl-params.conf;

        # Enter your domain name here or the IP address of the servers ethernet address:
        server_name 192.168.253.128;

        location / {
                proxy_pass http://localhost:8080/;
        }
}
server {
    listen 80;
    listen [::]:80;

    server_name 192.168.253.128;

    return 302 https://$server_name$request_uri;
}
```
Test the configuration for errors with:

```
sudo nginx -t
```

You should get something like this:

```
nginx: [warn] "ssl_stapling" ignored, issuer certificate not found for certificate "/etc/ssl/certs/nginx-selfsigned.crt"
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

The warning is completely normal because we use self signed certificates. See also the original documentation mentioned above!

## Configure the firewall

The final step is to configure the firewall to let the secure port pass.

```
# To see which apps are available for the firewall
sudo ufw app list

# Check the current status of ufw
sudo ufw status

# Add the secure port and remove port 80 from the firewall
sudo ufw allow 'Nginx Full'
sudo ufw delete allow 'Nginx HTTP'
```

Or when u used the instructions from part 6 of this tutorial:

```
sudo ufw delete allow http/tcp
sudo ufw delete allow https/tcp

sudo ufw status
```

## Reload nginx to make use of ssl

Reload the nginx configuration with the following command:

```
sudo nginx -s reload
```

Again, the warning ``nginx: [warn] "ssl_stapling" ignored, issuer certificate not found for certificate "/etc/ssl/certs/nginx-selfsigned.crt"`` is normal with self signed certificates.

Reload the nginx webserver with:

```
sudo systemctl restart nginx
```

## Test the connection

The final test is to see if the server is reachable over https and if the certificate is used.

Take note that because it is a self-signed certificate, you will still get an error in the browser but you can ignore this and proceed to your secured connection Freqtrade bot UI.

