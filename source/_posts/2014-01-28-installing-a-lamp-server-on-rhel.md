---
layout: post
title: Installing a LAMP Server on RHEL
date: 2014-01-28
categories:
  - SysAdmin
tags:
  - LAMP
  - RHEL
---

Recently, I signed up for the free tier of Amazon Web Services. I wanted to gain
some exposure to AWS and who can argue against having a free lab environment to
play with?

One of the first things I wanted to do was set up was a web server. I thought
I'd document the steps here for future reference. <!-- more --> In this case I
chose to use the free tier version of RHEL. (Accordingly, the instructions below
also apply to CentOS and other Red Hat based distros.) All of this is done as
ec2-user.

After connecting to the instance, the first thing to do is get the system up to
date.

    sudo yum update -y

Next up, install all packages and dependencies for Apache, MySQL and PHP. Also,
install the php-mysql package for MySQL support in any PHP apps.

    sudo yum groupinstall -y "Web Server" "MySQL Database Server" "MySQL Database Client" "PHP Support"
    sudo yum -y install php-mysql

If in your particular distro those names don't match up you can search for
the correct group name, e.g:

    sudo yum grouplist | grep MySQL

Now we can start Apache and configure it to start on boot.

    sudo service httpd start
    sudo chkconfig httpd on</pre>

Then allow port 80 traffic. Open up `/etc/sysconfig/iptables` and add the
following then restart iptables.

`-A INPUT -m state --state NEW -m tcp -p tcp --dport 80 -j ACCEPT`

    sudo service iptables restart

At this point you should be able to connect to the public DNS address of your
instance and see the Apache test page. If you can't connect, make sure your AWS
instance is in a security group allowing port 80.

If you connected successfully, set up a quick PHP test

    sudo echo "<?php phpinfo(); ?>" > /var/www/html/testphp.php

Try connecting to http://your.public.dns/testphp.php. You should see the PHP
information page. If so, go ahead and remove the testphp file as no one needs to
 see that info about your system.

    sudo rm /var/www/html/testphp.php

Next up, enable MySQL and set it to start on boot.

    sudo service mysqld start
    sudo chkconfig mysqld on

Finally, run `mysql_secure_installation` to set your root MySQL password and
remove all the insecure features.

    sudo mysql_secure_installation

The first question asks for the root password of your instance which by default
is blank so enter through that, then set a root password for the MySQL root user.

Now enter `Y` for the rest of the questions to:

  *  Remove anonymous users
  *  Disable remote root logins
  *  Remove the test database
  *  Reload all privilege tables and save changes

That's it! All done in just a few minutes. One last thing, dont forget to set up
permissions on `/var/www/` accordingly so that ec2-user has access to work with
any files in there.
