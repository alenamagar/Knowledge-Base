CSF
===

Installation of csf
--------------------

If there is any other firewall eg: APF, cphulk,etc then first disable it. No need to disable iptables.

.. code-block:: console

    # mkdir /root/audit
    # cd /root/audit
    # wget http://www.configserver.com/free/csf.tgz
    # tar -xzvf csf.tgz
    # cd csf
    # sh install.sh

To whitelist the ip in csf, add ip in following file

.. code-block:: console

    # vi /etc/csf/csf.allow

.. note::

    lfd can still block above whitelisted ip. Therefore, to tell lfd not to block ip, add ip in below file.

.. code-block:: console
    
    # vi /etc/csf/csf.ignore

Before doing anything to main configuration file, take a backup of it.

.. code-block:: console

    # cp /etc/csf/csf.conf /etc/csf/csf.conf_BAK

.. note::

    After installation of csf, lfd will not start. Since csf, is in TESTING mode, we have to manually disable it. 
    If it is not changed then you may get email like "lfd start failed". To disable TESTING mode go to /etc/csf/csf.conf
    and change TESTING = "1" to TESTING = "0".

.. note::
    
    Allow 5666 port for nagios monitoring
    Allow 3306 port for mysql 
    Allow 30000:50000 for ftp in passive mode (go to ftp setting, allow passive mode and restart the ftp service)

.. code-block:: console
    
    # vi /etc/csf/csf.conf

.. code-block:: bash
    
    TCP_IN = "20,21,22,25,53,80,110,143,443,465,587,993,995,2077,2078,2082,2083,2086,2087,2095,2096,2525,5666,3306,30000:35000"


To change the email address where you get the notification from csf. Insert your email address in following inside /etc/csf/csf.conf file

.. code-block:: console

    LF_ALERT_TO = ""

If you want to change the email address from then insert some email address in following inside /etc/csf/csf.conf 

.. code-block:: console
    
    LF_ALERT_FROM = ""

Change Virtual Memory Size from 200 to 300 in /etc/csf/csf.conf

.. code-block:: console

    PT_USERMEM = "300" 

Add following process so that csf will ignore it

.. code-block:: console

    # vi /etc/csf/csf.pignore
    cmd:spamd child (remove "#" sign)
    exe:/usr/local/cpanel/3rdparty/php/54/bin/php-cgi (Add this somewhere in file at the end of exe:...)

After doing all changes you need, restart the services

.. code-block:: console

    # csf -r
    # service lfd restart

To search for blocked-ip

.. code-block:: console

    # csf -g [ip-address]

To allow ip address

.. code-block:: console

    # csf -a [ip-address]

To block ip address

.. code-block:: console

    # csf -d [ip-address]

If you do not wish to get permanent block email notification then change 1 to 0 in following /etc/csf/csf.conf file

.. code-block:: console

    LF_EMAIL_ALERT = "0" 

If you want to remove csf and lfd service:
 
.. code-block:: console

    # cd /etc/csf
    # sh uninstall.sh

How to enable passive mode?

1. Add Passive Port range 30000-35000 to your Pureftp or Proftp configuration file

(i) Pureftpd
open ``/etc/pure-ftpd.conf``, and this line

.. code-block:: console

    PassivePortRange    30000 35000

(ii) ProFTP
Open ``/etc/proftpd.conf``, and add this line

.. code-block:: console

    PassivePorts    30000 35000

2.  Open the ports from 30000 – 35000 in your CSF firewall configuration file under TCP_IN

.. code-block:: console

    # vi /etc/csf/csf.conf

# Allow incoming TCP ports in /etc/csf/csf.conf file

.. code-block:: console

    TCP_IN = "20,21,22,25,53,80,110,30000:35000"

Then restart firewall and ftp server.

.. code-block:: console

    # csf -r
    # service pureftpd restart (or)
    # service proftpd restart


Whitelisting the running process

If you are receiving a number of ``"suspicious process"`` emails that mention the ``"spamd child"`` process CSF may identify it as a ``"suspicious process"``.  This can be avoided by adding the exclusion back into CSF.  Here's how:

Go to terminal and add binary full path ( /usr/local/cpanel/3rdparty/perl/514/bin/spamd) of the process to these two files

.. code-block:: console

    /etc/csf/csf.fignore
    /etc/csf/csf.pignore

To allow only ip to access port 22.

remove port 22 in ``/etc/csf/csf.conf`` under TCP_IN
insert following in ``/etc/csf/csf.allow``

.. code-block:: console

     tcp|in|d=22|d=ip-address # allow port 22 to only ip-address

Restart csf and lfd

.. code-block:: console

    # csf -r
    # service lfd restart

enabling the firewall

.. code-block:: console

    # csf -e

Disabling the firewall

.. code-block:: console

    # csf -x

Starting firewall / applying rules

.. code-block:: console

    # csf -s

Stopping firewall / flushing rules

.. code-block:: console

    # csf -f

Clustering in csf
-----------------

If one of the server detects a brute force attack originating from a certain IP address, the others don’t need to wait before they too are besieged. They can pass this information along to others to preempt attacks instead. The servers will be sharing white and black lists.

For us our master server will be 192.168.1.1, and
slave server will be 192.168.1.2 , 192.168.1.3

All the changes will be done in ``/etc/csf/csf.conf`` file

In master server

.. code-block:: console

    CLUSTER_SENDTO = “192.168.1.2,192.168.1.3″
    CLUSTER_RECVFROM = “192.168.1.2,192.168.1.3″
    CLUSTER_MASTER = “192.168.1.1”
    CLUSTER_PORT = “7777”
    CLUSTER_KEY = “01234567890123456789012345678901234567890123456789012345”
    CLUSTER_BLOCK = “1”
    CLUSTER_CONFIG = “0”

.. note::

    should be greater than 20 digit and less than 56 digit and same on all servers.

restart csf and lfd

.. code-block:: console

    #csf -r
    #service lfd restart

In slave servers

.. code-block:: console

    CLUSTER_SENDTO = “192.168.1.1″
    CLUSTER_RECVFROM = “192.168.1.1″
    CLUSTER_CONFIG = “1”
    CLUSTER_MASTER = “192.168.1.1” (optional)

restart csf and lfd

.. code-block:: console

    #csf -r
    #service lfd restart



When everything is done we need to verify if clustering is working or not

In master server type following:

.. code-block:: console
    
    #csf --cping

output should be like below:

.. code-block:: console

    Sent to 192.168.1.2
    Sent to 192.168.1.3


