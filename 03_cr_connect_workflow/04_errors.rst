=============================
Exceptions and Error Handling
=============================

CR Connect Workflow has a custom Exception defined in crc.api.common, and we use it throughout the backend.
It allows us to manage error reporting in one place, and format errors for the front-end.

--------
ApiError
--------

**ApiError** is defined in crc.api.common.

It has 2 required arguments; `code` and `message`.

By convention, **code** is short, all lower case, and separated by underscores.
It is meant to be read by code.

**Message** is meant to be read by a person, such as a user, configurator, or programmer.
It can be longer form, and should include information about the context of the error when possible.

The other arguments allow us to pass along additional information depending on the context.

.. code-block::

  class ApiError(Exception):
    def __init__(self, code, message, status_code=400,
                 file_name="", task_id="", task_name="", tag="", task_data = {}):
        self.status_code = status_code
        self.code = code  # a short consistent string describing the error.
        self.message = message  # A detailed message that provides more information.
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


------------------------
Helper Methods
------------------------

ApiError has 2 useful methods; **from_task** and **from_task_spec**.

If an error occurs in the context of a task or task_spec,
these methods allows us to include more information about the error.

TODO: task vs task spec

Notice that these are class methods.

from_task
---------

In addition to `code` and `message`, from_task requires a **task** argument.

.. code-block::

    @classmethod
    def from_task(cls, code, message, task, status_code=400):
        """Constructs an API Error with details pulled from the current task."""
        instance = cls(code, message, status_code=status_code)
        instance.task_id = task.task_spec.name or ""
        instance.task_name = task.task_spec.description or ""
        instance.file_name = task.workflow.spec.file or ""

        instance.task_data = task.data
        app.logger.error(message, exc_info=True)
        return instance


from_task_spec
--------------

In addition to `code` and `message`, from_task_spec requires a **task_spec** argument.

.. code-block::

    @classmethod
    def from_task_spec(cls, code, message, task_spec, status_code=400):
        """Constructs an API Error with details pulled from the current task."""
        instance = cls(code, message, status_code=status_code)
        instance.task_id = task_spec.name or ""
        instance.task_name = task_spec.description or ""
        if task_spec._wf_spec:
            instance.file_name = task_spec._wf_spec.file
        app.logger.error(message, exc_info=True)
        return instance



-------------
Example Usage
-------------

Using from_task
---------------

This example comes from WorkflowService._process_documentation in crc/services/workflow_service.py.

We trap two specific errors--TemplateError and TypeError, and raise an ApiError using from_task.
We also catch a general Exception error and log it.

.. code-block::


        try:
            template = Template(raw_doc)
            return template.render(**spiff_task.data)

        except jinja2.exceptions.TemplateError as ue:
            raise ApiError.from_task(code="template_error",
                                     message="Error processing template for task %s: %s" %
                                     (spiff_task.task_spec.name, str(ue)), task=spiff_task)

        except TypeError as te:
            raise ApiError.from_task(code="template_error",
                                     message="Error processing template for task %s: %s" %
                                     (spiff_task.task_spec.name, str(te)), task=spiff_task)

        except Exception as e:
            app.logger.error(str(e), exc_info=True)


Call ApiError directly
----------------------

In this example from crc/services/workflow_processor.py,
we call ApiError directly, without using from_task or from_task_spec.

.. code-block::

        except SyntaxError as e:
            raise ApiError('syntax_error',
                           f'Something is wrong with your python script '
                           f'please correct the following:'
                           f' {script}, {e.msg}')
        except NameError as e:
            raise ApiError('name_error',
                            f'something you are referencing does not exist:'
                            f' {script}, {e}')


--------------
ApiErrorSchema
--------------

CR Connect Workflow defines another class in crc.api.common that helps us format ApiError output as JSON so we can return it to the frontend

.. code-block::

  class ApiErrorSchema(ma.Schema):
    class Meta:
        fields = ("code", "message", "workflow_name", "file_name", "task_name", "task_id",
                  "task_data", "task_user", "hint")

In the class, we list the fields we want to include in the returned JSON.

We then use a hook provided by the api to format the output. This code is in crc/__init__.py

.. code-block::

  # Connexion Error handling
  def render_errors(exception):
      from crc.api.common import ApiError, ApiErrorSchema
      error = ApiError(code=exception.title, message=exception.detail, status_code=exception.status)
      return Response(ApiErrorSchema().dump(error), status=401, mimetype="application/json")

  connexion_app.add_error_handler(ProblemException, render_errors)


--------------------
Unhandled Exceptions
--------------------

CR Connect Workflow uses a feature of flask to capture unhandled exceptions.
In crc.api.common, we define a handler for InternalServerError and add a call to ApiError.

.. code-block::

    @app.errorhandler(InternalServerError)
    def handle_internal_server_error(e):
        original = getattr(e, "original_exception", None)
        api_error = ApiError(code='Internal Server Error (500)', message=str(original))
        response = ApiErrorSchema().dump(api_error)
        return response, 500


----
More
----

More about Python error handling: https://docs.python.org/3/tutorial/errors.html#handling-exceptions

More about Flask error handling: https://flask.palletsprojects.com/en/1.1.x/errorhandling/
