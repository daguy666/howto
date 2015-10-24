####(for 64bit use)

```$ rpm -Uvh http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm```

####(for 32bit use)

```$ rpm -Uvh http://dl.fedoraproject.org/pub/epel/6/i386/epel-release-6-8.noarch.rpm```

```$ rpm -Uvh http://rpms.famillecollet.com/enterprise/remi-release-6.rpm```


```$ yum -y install nagios nagios-plugins-all nagios-plugins-nrpe nrpe php httpd```

```$ chkconfig httpd on && chkconfig nagios on```

```$ service httpd start && service nagios start```

```$ htpasswd -c /etc/nagios/passwd nagiosadmin```

######login at:

```
http://{ipaddress}/nagios
```
