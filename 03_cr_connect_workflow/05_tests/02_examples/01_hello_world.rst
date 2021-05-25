Hello World
===========

One pattern we use to test new features is to create a workflow that uses the new feature.

Then, we can test the new feature by running the workflow and making assertions on the results.

Here is a simple test of a workflow.

Code
----

.. code-block::

    from tests.base_test import BaseTest


    class TestHelloWorld(BaseTest):

        def test_hello_world(self):
            workflow = self.create_workflow('hello_world')

            workflow_api = self.get_workflow_api(workflow)
            first_task = workflow_api.next_task

            self.complete_form(workflow, first_task, {'name': 'asdf'})

            workflow_api = self.get_workflow_api(workflow)
            second_task = workflow_api.next_task

            self.assertEqual('Hello asdf', second_task.documentation)

Setup
-----

The code starts with three things we do in any test file.


.. code-block::

    from tests.base_test import BaseTest

    class TestHelloWorld(BaseTest):

        def test_hello_world(self):

- We import BaseTest.
- We create a class that extends BaseTest. The class name must begin with **Test**.
- We define a method within our class with a name that begins with **test_**.


Our test class is named **TestHelloWorld** and our test method  is named **test_hello_world**.

You can have more than one test method in a test class.

Each test method must begin with **test_**.


Detail
------

Our test is relatively simple, but the method and approach is used in many of our tests.

- We create a workflow from the create_workflow method. This will be of type WorkflowModel.

.. code-block::

    workflow = self.create_workflow('hello_world')

.. Note::

    This means that we have a workflow named **hello_world.bpmn**,
    and it is inside a directory name **hello_world**,
    inside the /test/data directory.

    So, /tests/data/hello_world/hello_world.bpmn

- We create a workflow_api object from the workflow.


.. code-block::

    workflow_api = self.get_workflow_api(workflow)

A workflow_api object is similar to a workflow, but it has additional attributes for the frontend such as **status**, **next_task**, and **navigation**.

- We grab the next task.

.. code-block::

    first_task = workflow_api.next_task

This is the first task that needs a human interaction, usually a UserTask or ManualTask.

In this case, it is a UserTask with a form asking for a name.

- We complete a form by calling **complete_form**.

.. code-block::

    self.complete_form(workflow, first_task, {'name': 'asdf'})

We pass 3 items to complete_form; the workflow and task objects, and a dictionary.
The dictionary is how we pass data to the form.

The complete_form method is defined in BaseTest. Among other things, complete_form calls the
/workflow/{workflow_id}/task/{task_id}/data API endpoint.

- We again call get_workflow_api and get the next task.

.. code-block::

    workflow_api = self.get_workflow_api(workflow)
    second_task = workflow_api.next_task

This time next_task gives us the second task that requires human interaction.

It is a ManualTask that has some information in the Element Documentation.

- We check the value using an assert statement.

.. code-block::

    self.assertEqual('Hello asdf', second_task.documentation)



----------------
Example Workflow
----------------

Here is a workflow you can use in this test.

.. image:: /_static/05/02/process_hello_world.png

.. image:: /_static/05/02/task_get_name.png

.. image:: /_static/05/02/task_say_hello.png
