If you want to go a set further and want to add a proxy between your trading bot and the internet, then the solution below might be something for you.

## Set your freqtrade config.js to the default port 8080

For this setup, it is not necessary to use a obscure port on your server. Therefore just use the default Freqtrade port 8080

```
    "api_server": {
        "enabled": true,
        "listen_ip_address": "0.0.0.0",
        "listen_port": 8080,
        "verbosity": "error",
        "enable_openapi": false,
        "jwt_secret_key": "<USEARANDOMKEYFORSECURECONNECTION>",
        "CORS_origins": [],
        "username": "<YOURUSERNAME",
        "password": "<AVERYGOODPASSWORD>"
    },

```

## Install nginx

To install nginx, use the following commands:

```
sudo apt update

sudo apt install nginx
```

Check if nginx is running:

```
systemctl status nginx
```

Add the port to the firewall, otherwise you cannot connect to the webserver.

```
Sudo ufw allow http
```

Now you can check your browser:

```
http://172.16.65.131
```

## Configure nginx for a reverse proxy

Go to the directory:  
``cd /etc/nginx/conf.d``

And make a file that contains the redirecting rules:  
``sudo nano freq_proxy.conf``

Enter the content below in the file:

```
server { 
        listen 80;
        listen [::]:80;

        # Enter your domain name here or the IP address of the servers ethernet address:
        server_name 192.168.253.128;

        location / {
                proxy_pass http://localhost:8080/;
        }
}

```

It might be that a default.conf file exists in the conf.d directory. Rename this file to disable it.

```
ls

# If default.conf exists:

mv default.conf default.conf.disabled

```

Test the configuration for errors with:

```
sudo nginx -t
```

You should get something like this:

```
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successfull
```

Reload the nginx configuration with the following command:

```
sudo nginx -s reload
```

## Configure the firewall

This step is to configure the firewall to let port 80 pass.

Use the following commands to do this:

```
sudo ufw status

sudo ufw allow http/tcp

sudo ufw status
```

For https, use ``sudo ufw allow https/tcp``


Now browse to your bot on the default webserver port from another PC. 

```
http://192.168.253.128
```

