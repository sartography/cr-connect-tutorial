========
Postgres
========

We use Postgres for a database, and we deploy it with Docker.

.. Note::

    Before running the docker-compose command, you must set the DB_USER and DB_PASS environment variables.
    This is described in the :ref:`Environment Variables` section

------------------
Postgres Directory
------------------

Inside **/development/cr-connect-workflow**, there is a **postgres** directory.

The postgres directory contains scripts and Docker files to manage the Postgres install.

.. code-block::

    development/
        cr-connect-workflow/
            postgres/
                connect.sh
                docker-compose.yml
                docker-windows-compose.yml
                package-lock.json
                pg-init-scripts/
                start.sh
                stop.sh

We are interested in the **docker-compose.yml** file.

We have to tell docker-compose the location of this file.

--------------
Docker Compose
--------------

Make sure we are in the cr-connect-workflow directory.

.. code-block::

    cd /development/cr-connect-workflow

Then, run docker-compose and point to the correct file location.

.. code-block::

    docker-compose -f postgre/docker-compose.yml up

This should create and start up the databases.
There are four databases; **crc_dev**, **crc_test**, **pb**, and **pb_test**.

The databases ending in **_test** are used when we run tests.

----------
Initialize
----------

We now need to create the tables and add some example data.

Make sure we are in the correct directory.

.. code-block::

    cd /development/cr-connect-workflow

Then run the database commands.

.. code-block::

    pipenv run flask db upgrade

    pipenv run flask load-example-data


