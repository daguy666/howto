```
$ yum install ssmtp sendmail -y
```

```
$ /etc/init.d/sendmail stop ; chkconfig sendmail off
```

```
$ cp /etc/ssmtp/ssmtp.conf /etc/ssmtp/ssmtp.conf.org
```

```
$ cd /etc/ssmtp/
```

```
$ rm ssmtp.conf
```

```
$ vim /etc/ssmtp/ssmtp.conf
```

```
AuthUser={email address}
AuthPass={App Specific Password}
FromLineOverride=YES
mailhub=smtp.gmail.com:587
UseSTARTTLS=YES
```

```
$ which sendmail
```

```
$ which ssmtp
```

```
$ cp /usr/sbin/sendmail ~/sendmail.old
```

```
$ rm /usr/sbin/sendmail
```

```
$ cd /usr/sbin
```

```
$ ln -s /usr/sbin/ssmtp sendmail
```

```
echo "testing for nagios alerts"|mail -s "test nagiosalerts" emailid@example.com
```


######If mail command is not found then run 

```
$  yum install mailx -y 
```

######change contact file to match recipient 

```restart nagios {nagrestart}```
