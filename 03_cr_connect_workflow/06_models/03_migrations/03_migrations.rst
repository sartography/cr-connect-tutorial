==========
Migrations
==========

We manage migrations with the **flask db** command.

You can see the current revision.

.. code-block::

    flask db current

You can see the revision history order.

.. code-block::

    flask db history


You can run an upgrade.

.. code-block::

    flask db upgrade

You can back out of an upgrade.

.. code-block::

    flask db downgrade

To see more about the flask db command, use the help flag.

.. code-block::

    flask db --help


.. Note::

    We already used alembic, when we :ref:`initialized the database <Initialize>`.

    .. code-block::

        flask db upgrade

