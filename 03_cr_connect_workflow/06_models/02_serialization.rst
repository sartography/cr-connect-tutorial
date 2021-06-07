=============
Serialization
=============

Before sending data through the api, we have to serialize the Python objects.
We use Marshmallow for this.

In crc/models/data_store.py, we define **DataStoreModel**, the database model for our data store,
where we can save key/value pairs for studies, users, and files.


.. code-block::

    from flask_marshmallow.sqla import SQLAlchemyAutoSchema
    from sqlalchemy import func
    from crc import db

    class DataStoreModel(db.Model):
        __tablename__ = 'data_store'
        id = db.Column(db.Integer, primary_key=True)
        last_updated = db.Column(db.DateTime(timezone=True), server_default=func.now())
        key = db.Column(db.String, nullable=False)
        workflow_id = db.Column(db.Integer)
        study_id = db.Column(db.Integer, nullable=True)
        task_id = db.Column(db.String)
        spec_id = db.Column(db.String)
        user_id = db.Column(db.String, nullable=True)
        file_id = db.Column(db.Integer, db.ForeignKey('file.id'), nullable=True)
        value = db.Column(db.String)

In the same file, we also define **DataStoreSchema**, which we use to serialize the data.

.. code-block::

    class DataStoreSchema(SQLAlchemyAutoSchema):
        class Meta:
            model = DataStoreModel
            load_instance = True
            sqla_session = db.session


Note that this is the same way we serialized the response for ApiError in :ref:`api-error-schema-label`.

For more information about Marshmallow, see the `documentation <https://marshmallow-sqlalchemy.readthedocs.io/en/latest/>`_.
