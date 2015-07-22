How to install ioncube for WHMCS
================================

.. code-block:: console

    $ cd 
    $ mkdir audit
    $ cd audit

Download the latest ioncube loader from `Here <http://www.ioncube.com/loaders.php>`_ 

.. code-block:: console

    $ sudo wget http://downloads3.ioncube.com/loader_downloads/ioncube_loaders_lin_x86-64.tar.gz

Extract it

.. code-block:: console

    $ tar -xzvf ioncube_loaders_lin_x86-64.tar.gz

Copy the loader-wizard.php to /var/www/html

.. code-block:: console

    $ sudo cp ioncube/loader-wizard.php /var/www/html/

Copy ioncube module according to your php version$  to /usr/lib64/php/modules

.. code-block:: console

    $ sudo cp ioncube_loader_lin_5.6.so /usr/lib64/php/modules

Make a file inside /etc/php.d/ioncube.ini and paste below :

.. code-block:: console

    zend_extension = /usr/lib64/php/modules/ioncube_loader_lin_5.6.so

Now restart the httpd

.. code-block:: console

    $ sudo service httpd restart

Then, remove these files

.. code-block:: console

    $ sudo rm /var/www/html/loader-wizard.php
    $ sudo rm -rf audit/ioncube*

TroubleShooting
===============

Error 1
------

.. warning::

    Site error: the file /var/www/html/whmcs/install/install.php requires the ionCube PHP Loader ioncube_loader_lin_5.6.so to be installed by the site administrator.

If you get errors like above, look at error_log, for me above error tells me the error wwas related to  permission denied. If you have same error in err-r_log then give the proper permission to module or easy way out is move .so file elsewhere like below:

.. code-block:: console

    $ cd /usr/share
    $ mkdir ioncube
    $ mv /usr/lib64/php/modules/ioncube_loader_lin_5.6.so /usr/share/ioncube

Also make changes in /etc/php.ini file

.. code-block:: console

    zend_extension = /usr/share/ioncube/ioncube_loader_lin_5.6.so

restart the apache

.. code-block:: console

  $ sudo /etc/init.d/httpd restart


Error 2
-------

.. warning::

    # php -v
    PHP Fatal error: [ionCube Loader] The Loader must appear as the first entry in the php.ini file in Unknown on line 0

then it may be because there are two two defination for same module. Try to find the ``.ini`` file and inside it search for ``zend_extension``

If you find two entries, then remove one and restart apache
