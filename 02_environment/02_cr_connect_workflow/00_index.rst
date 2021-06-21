===================
CR Connect Workflow
===================

-------------------
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

