=============
Test a Script
=============

We can use the idea from the :ref:`Hello World` example, to test our script.

We can build a workflow that calls our script, then we can call that workflow in a test.

------
Script
------

Let's assume we have a script named **add_two_numbers.py**

Our script takes two numbers as arguments and returns a sum.
It raises ApiError if it does not receive two numbers.

.. code-block::

    from crc.api.common import ApiError
    from crc.scripts.script import Script


    class GetSum(Script):

        def get_description(self):
            pass

        def do_task_validate_only(self, task, study_id, workflow_id, *args, **kwargs):
            assert (isinstance(kwargs['x'], int) or isinstance(kwargs['x'], float))
            assert (isinstance(kwargs['y'], int) or isinstance(kwargs['y'], float))
            return self.do_task(task, study_id, workflow_id, *args, **kwargs)

        def do_task(self, task, study_id, workflow_id, *args, **kwargs):
            x = kwargs['x']
            y = kwargs['y']

            if (isinstance(x, int) or isinstance(x, float)) and (isinstance(y, int) or isinstance(y, float)):
                return x + y
            else:
                raise ApiError(code='bad_input',
                               message='Both x and y must be numbers.')



--------
Workflow
--------

Here is a workflow that calls our script.

First, we have a form with two inputs, 'x' and 'y'.

.. image:: /_static/03/05/02/test_script_workflow_1.png

Then, we pass x and y to our script.

.. image:: /_static/03/05/02/test_script_workflow_2.png

Finally, we output the sum in Element Documentation.

.. image:: /_static/03/05/02/test_script_workflow_3.png

.. Note::

    This example assumes we

    - created a directory inside **tests/data/** named **tutorial_call_script**
    - copied our workflow to this directory, and named the file **tutorial_call_script.bpmn**

    So

    .. code-block::

        tests/
            data/
                tutorial_call_script/
                    tutorial_call_script.bpmn


-----------
Test
-----------

Now, we can write a test for our script.

.. code-block::

    from tests.base_test import BaseTest


    class TestCallScript(BaseTest):

        def test_call_script(self):
            workflow = self.create_workflow('tutorial_call_script')
            workflow_api = self.get_workflow_api(workflow)
            first_task = workflow_api.next_task

            x = 2.1
            y = 3.7

            self.complete_form(workflow, first_task, {'x': x, 'y': y})

            second_task = workflow_api.next_task
            self.assertIn(second_task.documentation, f'{x} + {y}')
