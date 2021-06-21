=================
Working Directory
=================

-----------------
Working Directory
-----------------

Create a working directory in a suitable location.

.. code-block::

    mkdir development

    cd development

We will use **/development** as the top level of our example directory structure.

-----------------
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

