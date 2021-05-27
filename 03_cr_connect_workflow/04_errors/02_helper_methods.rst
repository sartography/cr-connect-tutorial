==============
Helper Methods
==============

ApiError has 2 useful methods; **from_task** and **from_task_spec**.

If an error occurs in the context of a task or task_spec,
these methods allows us to include more information about the error.

TODO: task vs task spec

Notice that these are class methods.

---------
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


--------------
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

