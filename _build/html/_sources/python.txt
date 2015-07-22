Python
======

Scan cryptoPHP in centos server
-------------------------------

Following script will search cryptoPHP infected themes/plugins based on *social.png* file.

.. note::

    To run this script you need to first run *updatedb* command.

.. code-block:: console

        #!/usr/bin/env python

	import os

	os.system("mkdir -p /root/audit/cryptoPHP/")
	os.system("locate social.png > /root/audit/cryptoPHP/list.txt")
	os.system("xargs md5sum < /root/audit/cryptoPHP/list.txt > /root/audit/cryptoPHP/possible-infected.txt")

	CRYPTOPHP_MD5SUM = ['048a54b0f740991a763c040f7dd67d2b', 'd3c9f64b8d1675f02aa833d83a5c6342', '3a2ca46ec07240b78097acc2965b352e', '4c641297fe142aea3fd1117cf80c2c8b', 'e27122ba785627fca79b4a19c8eea38b', '2640b3613223dbb3606d59aa8fc0465f', 'f5d6f783d39336ee30e17e1bc7f8c2ef','b75c82e68870115b45f6892bd23e72cf', '29576640791ac19308d3cd36fb3ba17b', 'b4764159901cbb6da443e789b775b928', '1ed6cc30f83ac867114f911892a01a2d', '325fc9442ae66d6ad8e5e71bb1129894','5b1d09f70dcfe7a3d687aaef136c18a1', '20671fafa76b2d2f4ba0d2690e3e07dc', '3249b669bb11f49a76850660411720e2', 'ffd91f505d56189819352093268216ad', 'b4b2c193f8af66b093ce1f1d284406a5', 'd11e6a54fba32fee9c69aabe9515e69d', 'f30dca4a681703178b4d1294425ae5f6']

	newfile = open("cryptoPHP-infected.txt", "w")

	with open('possible-infected.txt') as possible:
	        for line in possible:
	                if any(crypto in line for crypto in CRYPTOPHP_MD5SUM):
	                        newfile.write(line)
