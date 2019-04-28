---
layout: page
title: logDaemons
---

[return to os specifics](../os_specifics.html)


# Logging

### rsyslogd

* two flavors
  * syslogd (pre 2004)
  * rsyslogd ( forked @2004)
  * syslog-ng ( syslog next gen. Not on centos or debian yet )

#### Some commands

```
# get version
rsyslogd -v
```

http://rpms.adiscon.com/v8-stable

#### Rocket-Fast

System for Log processing

#### Basic Configuration

* rsyslogd
  */etc/rsyslog.conf

each line consists of
```facility.priority[;facility1.priority] /pathtofile```

eg

all info and above except for mail and authpriv to /var/log/messages
```
*.info;mail.none;authpriv.none /var/log/messages
```
all authpriv to /var/log/secure
```
authpriv.* /var/log/secure
```

##### facilities
* auth
* authpriv
* cron
* daemon
* lpr
* kern
* mail
* mark
* news
* security
* syslog
* user
* uucp
* local0-7

##### priorities
* debug
* info
* notice
* warn
* err
* crit
* alert
* emerg  

Most logs go to ```/var/log/``` or subdirectories therein.

Almost everything goes to  ```/var/log/messages```. Su and sudo events, and a couple of others go to ```/var/log/secure```.
The Kernel writes to ```/var/log/dmesg```

## An Aside - of .conf and .d

For many configurations, you will notice both a .conf file and a corresponding .d directory. For example logrotate.conf and logrotate.d. the conf file is used to store the basic configuration, and the .d directory stores additional extensions to the config. So the .conf file is processed first, then the .d directory contents are iterated over and applied next. Interesting....

#### logger

To log, you can use ```/usr/bin/logger``` from scripts to log to one of the logging services. The local0-7 are common targets for user scripts.

```
logger -p local5.info "Script Started"
logger -p local5.err "Script Error"
```
Of course, this is still going to go to ```/var/log/messages```. You can set the logger up to write to a local5 file by adding the following entry into the rsyslog.conf:

```
local5.* /var/log/local5.log
```
Of course, now we have to restart the rsyslogd

```
systemctl restart rsyslog.service
```

Now, lets back up the rsyslog.conf and remove all of those pesky comments using our friend sed.

```
sudo sed -i.$(date +%F) '/^#/d;/^$/d' /etc/rsyslog.conf
```

### logrotate

logrotate generally runs on a cron

```
logrotate /etc/logrotate.conf
```

#### On a daily cron

If we look in ```/etc/cron.daily```, we should find a logrotate wrapper, which is getting called on a daily basis by the cron.

#### Logrotate configuration

configuration is handled by ```/etc/logrotate.conf``` and ```/etc/logrotate.d/```

### contents of log configuration

```
filepath {
    size  # size  
    schedule # weekly, monthly...
    rotate <#> # number of copies to keep
    compress
}
```

EG

```
/var/log/local5 {
    size +30k
    weekly
    rotate 4
    compress
}
```

#### setting up logrotate for our local5

we could put the configuration directly in ```/etc/logrotate.conf`` however, we should probably simply create a file and place it in ```/etc/logrotate.d```, as this will be clearer.

We create ```/etc/logrotate.d/local5```. Note that it does not matter what it is called, but it makes sense to name it something sensible.


### journalctl
