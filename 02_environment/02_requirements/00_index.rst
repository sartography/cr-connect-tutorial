============
Requirements
============

CRConnect has three requirements; Python3, Angular, and Docker.

-------
Python3
-------

We use **pipenv** to manage our Python virtual environments and their dependencies.

Install pipenv
--------------

You can install pipenv with **pip3**.

Pip3 comes with Python3.

.. code-block::

    pip3 install pipenv

You can check to see whether pipenv is installed

.. code-block::

    pipenv --help

-------
Angular
-------

CRConnect requires **Angular**, which in turn requires **node.js**.

node.js
-------

You can download node.js from their `download page <https://nodejs.org/en/download/>`_

Angular
-------

Then you can install Angular using node.js.

.. code-block::

    npm install -g @angular/cli


------
Docker
------

We use Docker to manage our database.

Download Docker
---------------

You can learn more about Docker from their `Get Docker <https://docs.docker.com/get-docker/>`_ page.

docker-compose
--------------

You can test whether **docker-compose** is installed.

.. code-block::

    docker-compose -v

