=====================
Environment Variables
=====================

Our docker-compose script uses two environment variables; **DB_USER** and **DB_PASS**.

Managing environment variables is beyond the scope of this document, but...

Viewing environment variables
-----------------------------

You can see your current environment variables:

.. code-block::

    $ env

    TERM_PROGRAM=iTerm.app
    DB_PASS=crc_pass
    TERM=xterm-256color
    SHELL=/bin/bash
    CLICOLOR=true
    ...


Exporting environment variables
-------------------------------

You can set environment variables for the current session.

.. code-block::

    $ export DB_USER=crc_user
    $ export DB_PASS=crc_pass

When you **export** environment variables like this, they are available system wide, but only for your current shell session.

You can see them in your environment variables:

.. code-block::

    $ env |grep DB_
    DB_PASS=crc_pass
    DB_USER=crc_user


.env files
----------

Another option--and the one we use, is to set these environment variables in a **.env** file.

.. code-block::

    echo -e "DB_USER=crc_user\nDB_PASS=crc_pass" > .env

This creates a file with the two environment variables.

.. code-block::

    $ cat .env
    DB_USER=crc_user
    DB_PASS=crc_pass

When you create a **.env** file in a directory, the environment variables are limited to that directory.

Notice that they are not in your global environment.

.. code-block::

    $ env |grep DB_
