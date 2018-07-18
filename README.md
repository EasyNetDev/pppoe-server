# pppoe-server systemd service
------------------------------------

pppoe-server is a systemd service which will allow you to start/stop the PPPoE Server
on a specific interfaces.

The service is composed from 3 files:

```
/lib/systemd/system/pppoe-server@.service
/etc/default/pppoe-server
/usr/sbin/pppoe-server-config
```

The script pppoe-server-config will take the variables from /etc/default/pppoe-server will 
create a folder in /var/run/pppoe-server/ and it will create a **pppoe-server.ses** file. 
This file tracks all sessions for daemons.

Because for each interface we have to start a daemon the sessions must not overlap.

This file will keep a track for free sessions and also for used sessions and each time 
when a new daemon is started the script will return the necessary arguments for 
pppoe-server daemon.

Using the variables from the /etc/default/pppoe-server you can increase or decrease 
the number of the sessions per interface and also the total number of daemons.

**Attention!** If you change number of sessions and/or total number of daemons, 
please STOP all daemons, delete the file /var/run/pppoe-server/pppoe-server.ses 
and restart the daemons!

Also this script, if PPPOE_KERNELMODE is set in "auto", will try to find out if the 
PPPoE was build with kernel-mode support or not. This is important if you want to 
offload your CPU and move the PPPoE decapsulation into the kernel instead the userspace!

