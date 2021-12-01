## Some Freqtrade security tips

After a 'regular' installation via the install.sh script, Freqtrade does not have a graphical user interface or REST API that you can reach from another server.

The best security is NOT to use the API!

However, if you do use REST API or a UI, then be extra aware that you use a good API Server username and password.

U can use the following oneliner to create a random 12 character password:

```
date +%s | sha256sum | base64 | head -c 12 ; echo
```

Yuo can leave the config of the API like this if you want to use nginx as a proxy server.

```
    "api_server": {
        "enabled": true,
        "listen_ip_address": "127.0.0.1",
        "listen_port": 8080,
        "verbosity": "error",
        "enable_openapi": false,
        "jwt_secret_key": "<USEARANDOMKEYFORSECURECONNECTION>",
        "CORS_origins": [],
        "username": "<YOURUSERNAME",
        "password": "<AVERYGOODPASSWORD>"
    },

```

## Not using NGINX

If you do not want to use nginx, then consider using a obscure port for your web API.
```
    "api_server": {
        "enabled": true,
        "listen_ip_address": "127.0.0.1",
        "listen_port": 3665,
        "verbosity": "error",
        "enable_openapi": false,
        "jwt_secret_key": "<USEARANDOMKEYFORSECURECONNECTION>",
        "CORS_origins": [],
        "username": "<YOURUSERNAME",
        "password": "<AVERYGOODPASSWORD>"
    },

```

Change your firewall to allow traffic to that port:

```
sudo ufw allow 3665/tcp
```
