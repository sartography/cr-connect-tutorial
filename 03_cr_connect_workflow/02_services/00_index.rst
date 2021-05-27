.. _index-services:

==================
Building a Service
==================

Services are internal code related to a specific function. We have services that manage users and studies,
interact with Protocol Builder and the LDAP server, send emails, and make calls to SpiffWorkflow.

Services should not be called directly from outside the system. They should be called by scripts, the API, and other services.

.. toctree::
   :maxdepth: 2

   01_service
   02_script
