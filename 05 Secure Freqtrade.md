## Some Freqtrade security tips

After a 'regular' installation via the install.sh script, Freqtrade does not have a graphical user interface or REST API that you can reach from another server.

The best security is NOT to use the API!

However, if you do use REST API or a UI, then be extra aware that you use a good API Server username and password.

U can use the following oneliner to create a random 12 character password:

```
date +%s | sha256sum | base64 | head -c 12 ; echo
```

Also, use a obscure port for the interface.

```
    "api_server": {
        "enabled": true,
        "listen_ip_address": "0.0.0.0",
        "listen_port": 18834,
        "verbosity": "error",
        "enable_openapi": false,
        "jwt_secret_key": "<USEARANDOMKEYFORSECURECONNECTION>",
        "CORS_origins": [],
        "username": "<YOURUSERNAME",
        "password": "<AVERYGOODPASSWORD>"
    },

```

Allow the firewall to use the port you just configured

```
sudo ufw allow 18834/tcp
```

