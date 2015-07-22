
How to install Redis in Centos:
===============================

.. code-block:: console

    # yum install gcc make
    # yum -y install tcl

    # cd /opt
    # useradd redis
    # passwd redis


1. Download latest from `here <http://redis.io/download>`

.. code-block:: console

    # wget http://download.redis.io/releases/redis-3.0.2.tar.gz

2. To verify sha1sum `go here <https://github.com/antirez/redis-hashes/blob/master/README>

.. code-block:: console

    # sha1sum redis-3.0.2.tar.gz   (compare the output value to above given link redis version)

3. Extract the tar file

.. code-block:: console

    # tar -xzvf redis-3.0.2.tar.gz
    # mv redis-3.0.2 redis
    # cd redis
    # make 
     or
    # make install 

make : will create src folder with different executables
make install : will install the redis in ``/usr/local/bin``

make install command will create following executables will be available in ``/usr/local/bin/``
redis-benchmark  redis-check-aof  redis-check-dump  redis-cli  redis-sentinel  redis-server

.. code-block:: console

    # make test

4. Copy redis.conf file to /etc directory
now run

.. code-block:: console

    # cd src
    # ./redis-server ../redis.conf
    # ctrl + C

Make script to run it

.. code-block:: console

    # mkdir /scripts
    # cd scripts
    # vi start-redis
  
Add following

.. code-block:: console

    nohup runuser -l redis -c "/opt/redis/src/redis-server /opt/redis/redis.conf" > /tmp/redis.log &

go to ``/opt/redis/redis.conf``

uncomment requirepass and add long pass 

.. code-block:: console
    
    requirepass asmdgjsagdjkasgdkasgdkjasdgkasd

save it

start the script

.. code-block:: console

    # /scripts/start-redis
    # redis-cli

    > AUTH asmdgjsagdjkasgdkasgdkjasdgkasd
    OK
    >ping
    PONG
    >exit
