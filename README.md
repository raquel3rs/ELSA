ELSA 
====
###Enterprise Log Search and Archive

Step by step for ELSA install on Operative System CentOS 6.3.
___
####First get the install from the web
```$[root@localhost centos]# wget "http://enterprise-log-search-and-archive.googlecode.com/svn/trunk/elsa/contrib/install.sh"```

####Install the elsa node
```$[root@localhost centos]# sh -c "sh install.sh node"```
####Install the elsa web
```$[root@localhost centos]# sh -c "sh install.sh web"```

####Go edit rsyslog.conf 
```$[root@localhost centos]# vi /etc/rsyslog.conf```
####Add this to the end of file. 
Note that the IP address here written is the server that will be sending logs to syslog. In this case the local machine will be used.
`*.*@@127.0.0.1`

####See if IpTables is running. Allow the following ports for log nodes.
```$[root@localhost centos]# vi /etc/sysconfig/iptables```

####Add following rules to the iptables configuration
######Port 80 and/or 443 - for the web server

    -A INPUT -m state --state NEW -m tcp -p tcp --dport 80 -j ACCEPT
    -A INPUT -m state --state NEW -m tcp -p tcp --dport 443 -j ACCEPT

######Port 514 - for syslog

`-A INPUT -m state --state NEW -m tcp -p tcp --dport 514 -j ACCEPT`

######Port 3306 - for mysql

`-A INPUT -m state --state NEW -m tcp -p tcp --dport 3306 -j ACCEPT`

######Port 9306 - for Sphinx

`-A INPUT -m state --state NEW -m tcp -p tcp --dport 9306 -j ACCEPT`

####Restart Iptables
```$[root@localhost centos]# service iptables restart```

####Or stop Iptables if you don't need the extra care
```$[root@localhost centos]# service iptables stop```
___
**CONGRATS!** You have installed ELSA on your machine. Go to a browser, write localhost and you will see ELSA's web front end.

####Data location:
    Base Dir = "/usr/local"     Data Dir = "/data"
    Tmp Dir = "/tmp"            Local syslog Conf = "/etc/elsa/syslog-ng.conf
    MySQL node DB = "syslog"    MySQL Port = 3306

*Note:* if you have trouble, and ELSA displays errors, install de node or web again.
