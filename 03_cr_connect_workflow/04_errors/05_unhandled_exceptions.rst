====================
Unhandled Exceptions
====================

CR Connect Workflow uses a feature of flask to capture unhandled exceptions.
In crc.api.common, we define a handler for InternalServerError and add a call to ApiError.

.. code-block::

    @app.errorhandler(InternalServerError)
    def handle_internal_server_error(e):
        original = getattr(e, "original_exception", None)
        api_error = ApiError(code='Internal Server Error (500)', message=str(original))
        response = ApiErrorSchema().dump(api_error)
        return response, 500
