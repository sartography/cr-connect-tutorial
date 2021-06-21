========
Postgres
========

--------
Postgres
--------

We use Postgres for a database, and we deploy it with Docker.

Inside the **/development/cr-connect-workflow** directory,
there is a **postgres** directory containing scripts and Docker files to manage the Postgres install.

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

From the **/development/cr-connect-workflow/postgres** directory:

.. code-block::

    docker-compose up

This should create and start up the databases.
There are four databases; **crc_dev**, **crc_test**, **pb**, and **pb_test**.

The databases ending in **_test** are used when we run tests.

We now need to create the tables and add some example data.

From the **/development/cr-connect-workflow** directory

.. code-block::

    pipenv run flask db upgrade

    pipenv run flask load-example-data


