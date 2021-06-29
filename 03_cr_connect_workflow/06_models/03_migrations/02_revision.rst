=========
Revisions
=========

Alembic keeps track of database changes in **revision** files contained in the version directory.

A revision file is a python script containing alembic commands that modify the database.

Example
-------


A revision file starts with a **docstring** and some **import** statements.

The docstring can contain a message that is displayed when migrations are run.

.. code-block::

    """empty message

    Revision ID: 1fdd1bdb600e
    Revises: 17597692d0b0
    Create Date: 2020-06-17 16:44:16.427988

    """
    from alembic import op
    import sqlalchemy as sa


We then add some identifiers for alembic

.. code-block::

    # revision identifiers, used by Alembic.
    revision = '1fdd1bdb600e'
    down_revision = '17597692d0b0'
    branch_labels = None
    depends_on = None

The **revision** is the identifier for this migration.

The **down_revision** is the identifier for the previous migration.

Alembic uses the revision and down_revision to build the order of migration files for migrations.

Next, we add the **upgrade** and **downgrade** functions.

.. code-block::

    def upgrade():
        op.add_column('task_event', sa.Column('task_data', sa.JSON(), nullable=True))


    def downgrade():
        op.drop_column('task_event', 'task_data')

Migrations must include upgrade and downgrade functions

The upgrade function contains the new changes.

The downgrade function should undo those changes.

In this example, we use the alembic add_column and drop_column methods.

SQL Statements
--------------

Alembic can also run sql statements.

.. code-block::

    def upgrade():
        op.execute("alter table task_event alter column date type timestamp with time zone")
        op.execute("alter table workflow alter column last_updated type timestamp with time zone")


    def downgrade():
        op.execute("alter table task_event alter column date type timestamp without time zone")
        op.execute("alter table workflow alter column last_updated type timestamp without time zone")

