--------
ApiError
--------

Definition
----------

**ApiError** is defined in crc.api.common.

It has 2 required arguments; **code** and **message**.

By convention, `code` is short, all lower case, and separated by underscores.
It is meant to be read by code.

`Message` is meant to be read by a person, such as a user, configurator, or programmer.
It can be longer form, and should include information about the context of the error when possible.

The other arguments allow us to pass along additional information depending on the context.

.. code-block::

  class ApiError(Exception):
    def __init__(self, code, message, status_code=400,
                 file_name="", task_id="", task_name="", tag="", task_data = {}):
        self.code = code
        self.message = message
        self.status_code = status_code
        self.task_id = task_id or ""  # OPTIONAL:  The id of the task in the BPMN Diagram.
        self.task_name = task_name or ""  # OPTIONAL: The name of the task in the BPMN Diagram.
        self.file_name = file_name or ""  # OPTIONAL: The file that caused the error.
        self.tag = tag or ""  # OPTIONAL: The XML Tag that caused the issue.
        self.task_data = task_data or ""  # OPTIONAL: A snapshot of data connected to the task when error ocurred.
        if hasattr(g,'user'):
            user = g.user.uid
        else:
            user = 'Unknown'
        self.task_user = user


Usage
-----

To use ApiError, you first need to import it.

.. code-block::

  from crc.api.common import ApiError


Then you can use it in a code block like a try/except,

.. code-block::

  try:
      run_some_python_code()

  except SomeException as se:
      raise ApiError(code='my_code',
                     message=f'There was a problem with your code. Response was {se}.')


or an if/else.

.. code-block::

  model = some_sql_query()
  if model:
      do_something()
  else:
      raise AprError(code='bad_query',
                     message='Your query result was empty')