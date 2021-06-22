===============
Calling the API
===============

Sometimes, we want to call the API directly.
Flask provides a way to make requests from within the application.

--------
Overview
--------

Forms in a user task must have a form key. Here, we make sure our validation code catches missing form keys.


.. code-block::

    from tests.base_test import BaseTest
    import json


    class TestMissingFormKey(BaseTest):

        def test_missing_form_key(self):

            spec_model = self.load_test_spec('missing_form_key')
            rv = self.app.get('/v1.0/workflow-specification/%s/validate' % spec_model.id, headers=self.logged_in_headers())
            json_data = json.loads(rv.get_data(as_text=True))
            self.assertIn('code', json_data[0])
            self.assertEqual('missing_form_key', json_data[0]['code'])

--------
Detail
--------

First, we get a workflow spec

.. code-block::

    spec_model = self.load_test_spec('missing_form_key')

Then we call the api with the workflow spec id, and get a response.

.. code-block::

    rv = self.app.get('/v1.0/workflow-specification/%s/validate' % spec_model.id, headers=self.logged_in_headers())

We can grab the json from the response.

.. code-block::

    json_data = json.loads(rv.get_data(as_text=True))

We then assert that there was a missing form key.

.. code-block::

    self.assertEqual('missing_form_key', json_data[0]['code'])