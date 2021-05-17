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
