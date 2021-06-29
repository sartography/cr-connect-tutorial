======
Layout
======

The files for Alembic are kept inside the **migrations** directory

.. code-block::

    cr-connect-workflow/
        migrations/
            README
            env.py
            script.py.mako
            versions/
                0718ad13e5f3_.py
                62910318009f_.py
                bbf064082623_.py
                ...

**env.py** is used whenever you run a migration.

**script.py.mako** is a template for generating migration scripts.

The actual migration scripts are in the **versions** directory.

