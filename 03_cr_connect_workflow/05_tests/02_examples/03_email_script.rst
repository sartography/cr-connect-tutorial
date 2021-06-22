============
Email Script
============

The Hello World example gives us a great introduction to the basics of writing tests in CR Connect Workflow,
and the scaffolding available in BaseTest.

These next examples test the email script, and they each show us something more than the basics.

----------
Validation
----------

In this test, we run the validation script. This is the same as clicking on the shield in the Configurator.

It shows us how to call an API endpoint.

.. code-block::

    class TestEmailScript(BaseTest):

        def test_email_script_validation(self):
            # This validates scripts.email.do_task_validate_only
            # It also tests that we don't overwrite the default email_address with random text during validation
            # Otherwise json would have an error about parsing the email address
            self.load_example_data()
            spec_model = self.load_test_spec('email_script')
            rv = self.app.get('/v1.0/workflow-specification/%s/validate' % spec_model.id, headers=self.logged_in_headers())
            self.assertEqual([], rv.json)

---------------
with ... outbox
---------------

When testing email, we don't want to actually send emails.

Flask mail has a way to intercept the emails and show you the results using a context manager.

.. code-block::

    def test_email_script(self):
        with mail.record_messages() as outbox:

            self.assertEqual(0, len(outbox))

            workflow = self.create_workflow('email_script')

            workflow_api = self.get_workflow_api(workflow)
            first_task = workflow_api.next_task

            self.complete_form(workflow, first_task, {'subject': 'My Email Subject', 'recipients': 'test@example.com'})

            self.assertEqual(1, len(outbox))
            self.assertEqual('My Email Subject', outbox[0].subject)
            self.assertEqual(['test@example.com'], outbox[0].recipients)
            self.assertIn('Thank you for using this email example', outbox[0].body)

---------
Exception
---------

We can test for error conditions.

.. code-block::

    def test_bad_email_address_1(self):
        workflow = self.create_workflow('email_script')
        first_task = self.get_workflow_api(workflow).next_task

        with self.assertRaises(AssertionError):
            self.complete_form(workflow, first_task, {'recipients': 'test@example'})


--------
Add data
--------

We can add extra data for our test.

Here, we need a second user to add as an associate.

.. code-block::


    def test_email_script_associated(self):
        workflow = self.create_workflow('email_script')
        workflow_api = self.get_workflow_api(workflow)

        # Only dhf8r is in testing DB.
        # We want to test multiple associates, and lb3dp is already in testing LDAP
        self.create_user(uid='lb3dp', email='lb3dp@virginia.edu', display_name='Laura Barnes')
        StudyService.update_study_associates(workflow.study_id,
                                             [{'uid': 'dhf8r', 'role': 'Chief Bee Keeper', 'send_email': True, 'access': True},
                                              {'uid': 'lb3dp', 'role': 'Chief Cat Herder', 'send_email': True, 'access': True}])

        first_task = workflow_api.next_task

        with mail.record_messages() as outbox:
            self.complete_form(workflow, first_task, {'subject': 'My Test Subject', 'recipients': ['user@example.com', 'associated']})

            self.assertEqual(1, len(outbox))
            self.assertIn(outbox[0].recipients[0], ['user@example.com', 'dhf8r@virginia.edu', 'lb3dp@virginia.edu'])
            self.assertIn(outbox[0].recipients[1], ['user@example.com', 'dhf8r@virginia.edu', 'lb3dp@virginia.edu'])
            self.assertIn(outbox[0].recipients[2], ['user@example.com', 'dhf8r@virginia.edu', 'lb3dp@virginia.edu'])
