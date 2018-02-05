## Installing Cacti


### Installing and configuring Cacti with Cent OS 6.3

### Prerequisites
Before we can install Cacti, we must install the following packages.

- Apache: A Web server to display network graphs created by PHP and RRDTool.
- MySQL: A Database server to store cacti information.
- PHP: A script module to create graphs using RRDTool.
- PHP-SNMP: A PHP extension for SNMP to access data.
- NET-SNMP: A SNMP (Simple Network Management Protocol) is used to manage network.
- RRDTool: A database tool to manage and retrieve time series data like CPU load, Network Bandwidth etc.
- UnZip: A simple tool for extracting zip files


Once Cent OS 6.3 is installed login as “root” with the password you configured during the OS installation

- Installing Apache:
<br>

```
# yum install httpd httpd-devel -y
```

- Install MySQL:
<br>
```
# yum install mysql mysql-server -y
```

- Install PHP:
<br>
```
# yum install php-mysql php-pear php-common php-gd php-devel php php-mbstring php-cli php-mysql
```

- Install PHP-SNMP:
<br>
```
# yum install php-snmp
```

- Install NET-SNMP
<br>
```
# yum install net-snmp-utils p net-snmp-libs php-pear-Net-SMTP
```

- Install RRDTool
<br>
```
# yum install rrdtool
```

- Install UnZip:
<br>
```
# yum install unzip
```


### Now that all the required packages are installed, we must start their services running.

Start Apache
<br>
```
# service httpd start
```

Start MySQL
<br>
```
# service mysqld start
```

Start SNMP
<br>
```
# service snmpd start
```

Setup start up links for Apache, MySQL and SNMP
Apache
```
# chkconfig httpd on
```

MySQL
<br>
```
# chkconfig mysqld on
```

SNMP
<br>
```
# chkconfig snmpd on
```


###Add the Extra Packages for Enterprise Linux (EPEL) repository (64-Bit CentOS 6 only). This is where the Cacti installation is downloaded from.
```
# wget http://download.fedoraproject.org/pub/e ... noarch.rpm
```

```
# rpm -ivh epel-release-6-8.noarch.rpm
```

###OR
```
# yum install epel-release
```


Download and Install Cacti
<br>
```
# yum install cacti
```


Setup MySQL Server for Cacti
<br>
```
# mysqladmin -u root password Desired-Password-Here
```


Create MySQL Cacti Database
```
# mysql -u root -p
```

```
mysql> create database cacti;
mysql> GRANT ALL ON cacti.* TO cacti@localhost IDENTIFIED BY ‘Password-you-set-above’;
mysql> FLUSH privileges;
mysql> quit;
```


Setup Cacti Tables in MySQL
To being, we need to know the location of the cacti.sql file where the tables will be installed. Use the following command to show the location.

```
# rpm -ql cacti | grep cacti.sql
```

Sample Output (output may vary depending on version)
```
/usr/share/cacti-0.8.7d/cacti.sql
```


Now we need to install the tables into the cacti.sql file. Use the following command to do this, replace the green text for the location shown by the command above.

```
# mysql -u cacti -p cacti < /usr/share/cacti-0.8.7d/cacti.sql
```


Configure MySQL settings for Cacti
Open db.php with your preferred editor

```
# vim /etc/cacti/db.php
```

Make the following changes according to your config

```
/* make sure these values reflect your actual database/host/user/password */
$database_type = "mysql";
$database_default = "cacti";
$database_hostname = "localhost";
$database_username = "cacti";
$database_password = "your-password-here";
$database_port = "3306";
$database_ssl = false;
```

Configure Apache Server for Cacti Installation
You need to allow access to Cacti from the ipranges you require. By default I choose to allow access from all IP addresses. This can be viewd as an unsecure option and you may wish to follow a different path. (Google is your friend).

Open /etc/httpd/conf.d/cacti.conf with your preferred editor

```
# vim /etc/httpd/conf.d/cacti.conf
```

Add the following section at the bottom of the config file

```
Alias /cacti /usr/share/cacti

<Directory /usr/share/cacti/>
Order Deny,Allow
Deny from none
Allow from all
</Directory>
```

In order for this change to take effect, Apache must be restarted. Issue the following command to restart Apache

```
# service httpd restart
```

Setting Cron for Cacti
Open the Cacti cron file and and uncomment the line to enable the poller.php to run every5 mins.

```
#vim /etc/cron.d/cacti
```

Delete or uncomment out the # in the following line

```
#*/5 * * * * cacti /usr/bin/php /usr/share/cacti/poller.php > /dev/null 2>&1
```

Configure firewall to allow access to Cacti from the web
Open the iptables file

```
# vim /etc/sysconfig/iptables
```

Add the following line 
```
# -A INPUT -m state --state NEW -m tcp -p tcp --dport 80 -j ACCEPT
```

Restart iptables
```
# service iptables restart
```


Complete Cacti Installation via web installer
Open ```http://SERVER-IP/cacti``` to view the web installer.

