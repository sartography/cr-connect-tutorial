.. _index-cr-connect-workflow:

===================
CR Connect Workflow
===================

CR-Connect-Workflow is the backend for CR-Connect.

It has an `API` that takes requests from the front end,
and returns JSON responses.

There are SQLAlchemy database `models` for data persistence. We use a Postgres database.

It has `services` that offload most of the work for the API. These services can make calls to other services,
to SpiffWorkflow--our workflow engine, or to external data sources.

There are `scripts` which can be called from workflows. These scripts can call services or run standard Python.
They are a primary way of extending workflow capabilities.

.. image:: /_static/cr_connect_workflow.png

We will take a simple example of calling an external data source,
and use it to learn about the different parts of the CR Connect Workflow code base.


.. toctree::
   :maxdepth: 2

   01_scripts
   02_services
   03_api
   04_errors/00_index.rst
