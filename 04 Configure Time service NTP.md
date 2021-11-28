## Setting the correct timezone

First check the date and timezone of your system

```
sudo timedatectl
```

Check the timezone and if the ntp service is activated and synchronized.

Use the following command to see the symlink to the configured system timezone:

```
sudo ln -l /etc/localtime
```

or

```
sudo cat /etc/timezone
```

To check how your own timezone is called, use the next command:

```
sudo timedatectl list-timezones
```

Save your location so you can use it in the next command.  
In my case: ``Europe/Amsterdam``

```
sudo timedatectl set-timezone Europe/Amsterdam
```

Now check the configured timezone once again with:

```
sudo timedatectl
```

## NTP synchronisation

It's important that the bot's time is synchronised with a time server.

For Europe, you can see the timeservers here: https://www.pool.ntp.org/zone/europe

```
sudo nano /etc/systemd/timesyncd.conf
```

And add or uncomment the following lines after \[Time\] statement, as illustrated in the below excerpt:

```
[Time]
NTP=0.nl.pool.ntp.org  
FallbackNTP=0.europe.pool.ntp.org 1.europe.pool.ntp.org
RootDistanceMaxSec=5
PollIntervalMinSec=32
PollIntervalMaxSec=2048
```

After editing the file, issue the timedatectl command to activate the NTP client build in systemd.

```
sudo timedatectl set-ntp true 
sudo timedatectl status
```

Restart and check the service:

```
systemctl restart systemd-timesyncd
systemctl status systemd-timesyncd
```

If not active use:

```
systemctl start systemd-timesyncd
systemctl enable systemd-timesyncd
```

Afterwards, issue date command in order to display your system clock.

```
dcd@debian11:~$ date
Fri 29 May 16:16:08 CEST 2020
```


