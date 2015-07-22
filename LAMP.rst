Installation of LAMP on centos 6.6
==================================

Install wget
------------

.. code-block:: console

    $ sudo yum install wget

Install Apache
--------------

To install apache,

.. code-block:: console

    $ sudo yum install httpd

Once it installs, you can start apache running on your VPS:

.. code-block:: console

    $ sudo service httpd start
    $ sudo chkconfig httpd on

Edit the ``#ServerName www.example.com:80`` and put your hostname instead of ``www.example.com`` in ``httpd.conf`` file

.. code-block:: console

    $ sudo vi /etc/httpd/conf/httpd.conf

Install MySQL 5.6
-----------------

Download the yum repo rpm package.

.. code-block:: console

    $ wget http://repo.mysql.com/mysql-community-release-el6-5.noarch.rpm

Install the downloaded rpm package.

.. code-block:: console

    $ rpm -ivh mysql-community-release-el6-5.noarch.rpm


Install the mysql server

.. code-block:: console

    $ yum install mysql-server

After installation start the mysql server

.. code-block:: console

    $ /etc/init.d/mysqld start
    $ sudo chkconfig mysqld on

MySQL server is just installed it has blank mysql root password. To reset the mysql root password.

.. code-block:: console

    $ sudo mysql_secure_installation

Install PHP 5.6
---------------

Download the repo rpm package

.. code-block:: console

    $ sudo rpm -Uvh https://mirror.webtatic.com/yum/el6/latest.rpm

Install the php with the required extension

.. code-block:: console

    $ sudo yum install -y php56w php56w-opcache php56w-xml php56w-mcrypt php56w-gd php56w-devel php56w-mysql php56w-intl php56w-mbstring


Restart Apache:

.. code-block:: console

    $ sudo service httpd restart

To verify that PHP 5.6 is installed:

.. code-block:: console

    $ php -v

For testing php test page

.. code-block:: console

    $ sudo vi /var/www/html/info.php

Add in the following line:

.. code-block:: console

    <?php
     phpinfo();
    ?>

Then save it and restart apache.

.. code-block:: console

    $ sudo service httpd restart

PHP Modules
-----------

PHP also has a variety of useful libraries and modules that you can add onto your server. You can see the libraries that are available by typing:

.. code-block:: console

    $ sudo yum search php-

To see more details about what each module does, type the following command into terminal, replacing the name of the module with whatever library you want to learn about.

.. code-block:: console

    $ sudo yum info name of the module

Once you decide to install the module, type:

.. code-block:: console

    $ sudo yum install name of the module

You can install multiple libraries at once by separating the name of each module with a space.

Now LAMP stack is installed!

Allow port 80

.. code-block:: console

    $ sudo vi /etc/sysconfig/iptables

and add this line

.. code-block:: console

    -A INPUT -m state --state NEW -m tcp -p tcp --dport 80 -j ACCEPT

Visit site at : `http://ip-address`
