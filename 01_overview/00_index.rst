.. _index-overview:

======================
Introduction
======================

CR Connect is designed to run in a Linux environment, but you can develop with any system running Python.

Currently, CR Connect requires Python 3.8.

.. Note::

    Before starting, you must be able to run **python3** and **pip3** on your system.

-----------
Big Picture
-----------

CR Connect is a collection of code and data bases.

.. image:: /_static/CR_Connect_stack.png


---------------
CR Connect BPMN
---------------

Low code editor/Workflow modeler.

Written in Angular.

Creates BPMN and DMN XML.

Can upload supporting documents.

https://github.com/sartography/cr-connect-bpmn


-------------------
CR Connect Frontend
-------------------

Interface for users.

Written in Angular.

Runs workflows.

Talks to the API.

https://github.com/sartography/cr-connect-frontend


-------------------
CR Connect Workflow
-------------------

The back end.

Written in Python.

Takes requests from CR Connect Frontend and CR Connect BPMN.

Uses the SpiffWorkflow workflow engine.

https://github.com/sartography/cr-connect-workflow


-------------
SpiffWorkflow
-------------

Workflow engine.

Written in Python.

Runs BPMN workflows.

https://github.com/sartography/SpiffWorkflow


---------------------
Protocol Builder Mock
---------------------

A mock-up of Protocol Builder.

Written in Python, HTML, Javascript.

Only meant to be used during development phase.

https://github.com/sartography/protocol-builder-mock


---------------------
Sartography Libraries
---------------------
Libraries shared by cr-connect-frontend and cr-connect-bpmn.

Written in Angular.

https://github.com/sartography/sartography-libraries


---------
Databases
---------

Both CR Connect and PB Mock use Postgres for a database.
