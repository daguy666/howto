## Setting up an Configuring Splunk Fowarder (6.02) for CentOs 


###### Download the correct splunk forwarder for your system
[Splunk Universal Fowarder](https://www.splunk.com/download/universalforwarder)

```
# rpm -Uvh splunkforwarder-6.0.2-196940.i386.rpm

warning: splunkforwarder-6.0.2-196940.i386.rpm: Header V3 DSA/SHA1 Signature, key ID 653fb112: NOKEY
Preparing...                ########################################### [100%]
   1:splunkforwarder        ########################################### [100%]
   complete
```

###### Edit user info for authentication
```
/opt/splunkforwarder/bin # ./splunk edit user admin -password <NEWPASS> -auth
```

###### Configure forwarding:
```
/opt/splunkforwarder/bin # ./splunk add forward-server {ip of server}:9997

   Added forwarding to: 192.168.1.11:9997.

   /opt/splunkforwarder/bin #   ./splunk list forward-server
   Active forwards:
    None
    Configured but inactive forwards:
        192.168.1.11:9997
```


###### Added monitor of ```/var/log```

```
/opt/splunkforwarder/bin # ./splunk add monitor /var/log
```

###### Enables boot-start

```
/opt/splunkforwarder/bin # ./splunk enable boot-start
Init script installed at /etc/init.d/splunk.
Init script is configured to run at boot.
```

###### chkconfig splunk on

```
/opt/splunkforwarder/bin # chkconfig splunk on
```
