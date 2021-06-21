.. _index-development-environment:

=======================
Environment
=======================

In this section, we install a development environment for CRConnect.
This includes four code bases and

This section covers how to install a development environment for developing CR Connect.

.. Note::

    To set up your environment, you need Python 3, node.js, Angular, and Docker installed.

    In particular, you must be able to run

    .. code-block::

        $ pipenv

    .. code-block::

        $ npm

    .. code-block::

        $ docker-compose


What follows is a step by step process for installing the code bases.

.. toctree::
   :maxdepth: 2

   01_working_directory/00_index
   02_cr_connect_workflow/00_index
   03_environment_variables/00_index
   04_postgres/00_index
   05_pb_mock/00_index
   06_cr_connect_bpmn/00_index
   07_cr_connect_frontend/00_index
   08_running_services/00_index


------
Pipenv
------

We need to install **pipenv**, which we use to maintain our Python packages and virtual environment.

Use pip3, which comes with Python3. (You can run this command from any directory.)

.. code-block::

    pip3 install pipenv

-----------
Run Backend
-----------

At this point we should be able to run the backend.

.. code-block::

    pipenv run python run.py

You can test by loading the web page at http://localhost:5000/v1.0/ui/#/


----
URLs
----

You should now be able to reach all four locations:

- `API <http://localhost:5000/v1.0/ui/>`_

- `Dashboard <http://localhost:5004/app/home>`_

- `Configurator <http://localhost:5002/bpmn/home>`_

- `PB Mock <http://localhost:5001>`_
