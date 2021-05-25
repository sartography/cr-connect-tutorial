.. _index-tests:

=============
Writing Tests
=============

CR Connect Workflow uses **pytest** for testing. It also has a **BaseTest** class.

`BaseTest` gives you access to a test database, and has fixtures for loading sample data, as well as working with users, studies, and workflows.

Our general approach is to have full coverage at the unit-test level.

.. toctree::
   :maxdepth: 2

   01_layout
   02_examples/00_index.rst
