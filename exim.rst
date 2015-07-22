Exim CheatSheet
===============

1. To check the number of emails present in the queue:

.. code-block:: console

    # exim -bpc

2. To check the emails present in the queue with the mail id and sender ID:

.. code-block:: console

    # exim -bp
    # exim -bp | less

3. To view the header of a particular email using mail ID:

.. code-block:: console

    # exim -MvH mail_id

4.  To view the body of a particular email using mail ID:

.. code-block:: console

    # exim -Mvb mail_id 

5. To view a message's logs:

.. code-block:: console

    # exim -Mvl mail_id

6. To trace path:

.. code-block:: console
 
    # exim -d -bt user@domain.com

7. To get sorted list of email sender in exim queue:

.. code-block:: console

    # exim -bpr | grep "<" | awk {'print $4'} |cut -d "<" -f 2 | cut -d ">" -f 1 | sort -n | uniq -c| sort -n

8. To check the script that will originate spam mails:

.. code-block:: console

    # grep "cwd=" /var/log/exim_mainlog|awk '{for(i=1;i<=10;i++){print $i}}'|sort| uniq -c|grep cwd|sort -n

9. If we need to find out exact spamming script. To do this, run following command:

.. code-block:: console

    # ps auxwwwe | grep user | grep --color=always "/home/user/public_html/templates/" | head

10.  To delete the emails of a specific user:

.. code-block:: console

    # grep -lr 'user@domain.com' /var/spool/exim/input/ | sed -e 's/^.*\/\([a-zA-Z0-9-]*\)-[DH]$/\1/g' | xargs exim -Mrm
    # exim -bp | grep "user_email-account" | awk '{print $3}' | xargs exim -Mrm

11. To delete Frozen emails from the email queue:

.. code-block:: console

    # grep -R -l '*** Frozen' /var/spool/exim/msglog/*|cut -b26-|xargs exim -Mrm
    # exim -bp| grep frozen | awk '{print $3}'| xargs exim -Mrm
    # exiqgrep -z -i | xargs exim -Mrm 

12.  To delete Spam emails from the email queue:

.. code-block:: console

    # grep -R -l [SPAM] /var/spool/exim/msglog/*|cut -b26-|xargs exim -Mrm

13. To check the no. of frozen mails:

.. code-block:: console

    # exiqgrep -z -c

14. To check exim logs:

.. code-block:: console

    # tail -f /var/log/exim_mainlog

15. Force delivery of one message:

.. code-block:: console

    # exim -M mail_id

16. Force another queue run:

.. code-block:: console

    # exim -qf

17. Force another queue run and attempt to flush frozen messages:

.. code-block:: console

    # exim -qff

18. To check if there are frozen emails:

.. code-block:: console

    # exim -bp |awk '/fr[o]zen/ {print}' 

19. To clear just one email:

.. code-block:: console

    # exim -Mrm mail_id

20. Check the subjects of the emails:

.. code-block:: console

    # exiqgrep -i |awk '{ print "exim -Mvh "$1 }' |sh |grep -i Subject

21. Delete the email which content some string in the message body

.. code-block:: console

    # grep -lr 'photos to album' /var/spool/exim/input/ | sed -e 's/^.*\/\([a-zA-Z0-9-]*\)-[DH]$/\1/g' | xargs exim -Mrm
