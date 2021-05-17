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
