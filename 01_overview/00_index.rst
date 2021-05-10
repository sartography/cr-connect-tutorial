.. _index-overview:

======================
Introduction
======================

This is an overview for the CR Connect Development Training for UVA. It will focus on building the development environment and working with the CR Connect Workflow code base.

The examples are Unix based, but you can develop in a Windows environment.

Currently, CR Connect requires Python 3.8.

Before starting class, you should know how to:

- run python 3 on your system, and
- run pip, the python package installer, and
- install pipenv using pip

We will use pipenv to manage the CR Connect Workflow virtual environment and code requirements.

----------
CR Connect
----------

CR Connect is a collection of three code bases; **CR Connect Front End**, **CR Connect BPMN**, and **CR Connect Workflow**, and a database for persistent storage.

CR Connect Front End and CR Connect BPMN use the **Sartography Utilities** library.

CR Connect Workflow uses the **SpiffWorkflow** workflow engine.

There is a mock of **Protocol Builder** for development.

We use Postgres for a database.

.. image:: /_static/CR_Connect_stack.png


----------
Code Bases
----------

CR Connect Workflow
-------------------
API written in Python. Takes requests from CR Connect Frontend and CR Connect BPMN.
Currently, this document focuses on CR Connect Workflow.

https://github.com/sartography/cr-connect-workflow


SpiffWorkflow
-------------
Workflow engine implemented in Python that runs BPMN workflows.

https://github.com/sartography/SpiffWorkflow


CR Connect Frontend
-------------------
Front end for users. Talks to the API. Written in Angular

https://github.com/sartography/cr-connect-frontend


CR Connect BPMN
---------------
Workflow modeler. Creates BPMN and DMN XML. Can upload supporting documents. Written in Angular

https://github.com/sartography/cr-connect-bpmn


Protocol Builder Mock
---------------------
A mock-up of Protocol Builder. Only meant to be used during development phase. Python and HTML

https://github.com/sartography/protocol-builder-mock


Sartography Libraries
---------------------
Libraries shared by cr-connect-frontend and cr-connect-bpmn. Written in Angular.

https://github.com/sartography/sartography-libraries

