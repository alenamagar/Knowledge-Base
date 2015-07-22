Postfix CheatSheet
==================

count mail queue number

.. code-block:: console

    # mailq | grep -c "^[A-Z0-9]"

delete mailq from domain address

.. code-block:: console

    # postqueue -p | tail -n +2 | awk 'BEGIN { RS = "" } /@domain\.com/ { print $1 }' | tr -d '*!' | postsuper -d -

To delete all email in the queue From: a specific email address run this command as root:

.. code-block:: console

    # postqueue -p | tail -n +2 | awk 'BEGIN { RS = "" } /username@domain\.com/ { print $1 }' | tr -d '*!' | postsuper -d -

Use following perl script to delete depending upon username or domain name:

.. code-block:: console

  #!/usr/bin/perl
   
  $REGEXP = shift || die "no email-adress given (regexp-style, e.g. bl.*\@yahoo.com)!";
   
  @data = qx</usr/sbin/postqueue -p>;
  for (@data) {
    if (/^(\w+)(\*|\!)?\s/) {
       $queue_id = $1;
    }
    if($queue_id) {
      if (/$REGEXP/i) {
        $Q{$queue_id} = 1;
        $queue_id = "";
      }
    }
  }
   
  #open(POSTSUPER,"|cat") || die "couldn't open postsuper" ;
  open(POSTSUPER,"|postsuper -d -") || die "couldn't open postsuper" ;
   
  foreach (keys %Q) {
    print POSTSUPER "$_\n";
  };
  close(POSTSUPER);

Usage:

.. code-block:: console

    ./scriptname.pl *username*
    ./scriptname.pl *domainname*
