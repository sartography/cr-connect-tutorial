===========
SQL Alchemy
===========

-----
Setup
-----

We setup the database in crc/__init__.py.

.. code-block::

    app = connexion_app.app

    ...

    db = SQLAlchemy(app)

    session = db.session

We can then import **db** or **session**, depending on our needs.

-------
Example
-------

We log emails sent by the system. Here is the code for the email model.

This code is in crc/models/email.py.

.. code-block::

    from crc import db
    from crc.models.study import StudyModel


    class EmailModel(db.Model):
        __tablename__ = 'email'
        id = db.Column(db.Integer, primary_key=True)
        subject = db.Column(db.String)
        sender = db.Column(db.String)
        recipients = db.Column(db.String)
        content = db.Column(db.String)
        content_html = db.Column(db.String)
        study_id = db.Column(db.Integer, db.ForeignKey(StudyModel.id), nullable=True)
        study = db.relationship(StudyModel)


We define our model in a class that extends **db.Model**.

Like most of our models, we have a primary key of **id**.

The important columns for the email are **subject**, **sender**, **recipients**, **content**,
and **content_html**, which are all defined as strings.

Notice that there is both a **study_id** column that is a **Foreign Key**,
and a **study** column that is a **Relationship**.

This model likely started with just study_id, and study was added later.
But, we kept the study_id column for backwards compatibility.


-----
Usage
-----

An example of using this model can be found in crc/services/email_service.py

.. code-block::

    def add_email(subject, sender, recipients, content, content_html, cc=None, study_id=None):

        ...

        email_model = EmailModel(subject=subject, sender=sender, recipients=str(recipients),
                                 content=content, content_html=content_html, study=study)

        ...

        db.session.add(email_model)
        db.session.commit()

To add a record to the database,

- instantiate an instance of the model object
- add the instance to the session
- commit the change


For more information about SQLAlchemy, see the `documentation <https://docs.sqlalchemy.org/en/14/>`_.

