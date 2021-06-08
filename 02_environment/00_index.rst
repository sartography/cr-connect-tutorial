.. _index-development-environment:

=======================
Environment
=======================

This section covers how to install a development environment for developing CR Connect.

.. Note::

    To set up your environment, you need Python 3, node.js, Angular, and Docker installed.


Here is a step by step process for installing the code bases.

Working Directory
-----------------

Create a working directory in a suitable location.

.. code-block::

    mkdir development

    cd development

We will use **/development** as the top level of our example directory structure.

Clone from github
-----------------

As we mentioned in the introduction, CR Connect is a collection of code bases.

For development, we have to clone 4 git repositories; CR Connect Workflow, CR Connect BPMN,
CR Connect Frontend, and PB Mock.


From inside the **/development** directory.

.. code-block::

    git clone https://github.com/sartography/cr-connect-workflow.git

    git clone https://github.com/sartography/cr-connect-bpmn.git

    git clone https://github.com/sartography/cr-connect-frontend.git

    git clone https://github.com/sartography/protocol-builder-mock.git

This creates four directories.

.. code-block::

    $ ls -l
    total 0
    drwxr-xr-x  23 michaelc  staff  736 Jun  7 16:04 cr-connect-bpmn
    drwxr-xr-x  25 michaelc  staff  800 Jun  7 16:04 cr-connect-frontend
    drwxr-xr-x  31 michaelc  staff  992 Jun  7 16:04 cr-connect-workflow
    drwxr-xr-x  22 michaelc  staff  704 Jun  7 16:04 protocol-builder-mock

Pipenv
------

We need to install **pipenv**, which we use to maintain our Python packages and virtual environment.

Use pip3, which comes with Python3. (You can run this command from any directory.)

.. code-block::

    pip3 install pipenv

CR Connect Workflow
-------------------

Next, we will use **pipenv** to set up CR Connect Workflow.

From inside the **/development/cr-connect-workflow** directory:

.. code-block::

    pipenv install --dev

Pipenv creates a Python virtual environment, and installs all our dependencies.

You can see the location of the virtual environment

.. code-block::

    pipenv --venv

You should see a path with the location to your virtual environment, something like this.

.. code-block::

    ~/.local/share/virtualenvs/cr-connect-AYWZ0UOE

.. Note::

    If you use Visual Studio on Windows, and have trouble installing the python-Levenshtein package,
    you might need to download the build tools.
    https://visualstudio.microsoft.com/thank-you-downloading-visual-studio/?sku=BuildTools&rel=16

Postgres
--------

We use Postgres for a database, and we deploy it with Docker.

Inside the **/development/cr-connect-workflow** directory,
there is a **postgres** directory containing scripts and Docker files to manage the Postgres install.

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


Run Backend
-----------

At this point we should be able to run the backend.

.. code-block::

    pipenv run python run.py

You can test by loading the web page at http://localhost:5000/v1.0/ui/#/


PB Mock
-------

Installing PB Mock is similar to CR Connect Workflow.

From the **/development/protocol-builder-mock** directory:

.. code-block::

    pipenv install --dev

    pipenv run python run.py


CR Connect BPMN
---------------

CR Connect BPMN is written in Angular, so the install is different from CR Connect Workflow and PB Mock.

From the **/development/cr-connect-bpmn** directory:

.. code-block::

    npm install

    ng serve


CR Connect Frontend
-------------------

CR Connect Frontend is also Angular, and works just like CR Connect BPMN.

From the **/development/cr-connect-frontend** directory:

.. code-block::

    npm install

    ng serve

URLs
----

You should now be able to reach all four locations:

- `API <http://localhost:5000/v1.0/ui/>`_

- `Dashboard <http://localhost:5004/app/home>`_

- `Configurator <http://localhost:5002/bpmn/home>`_

- `PB Mock <http://localhost:5001>`_
