-------
api.yml
-------

The API endpoints are defined in `api.yml` in the `paths` section.

Endpoints all have a `path`. Our path will be `get_cards`.
Endpoints can have `parameters`. Our parameters are `cards` and `decks`.
Our parameters are included as part of the query.
They are integers, and are not required.

The request type for our endpoint is `GET`.
We tell the endpoint what Python code to execute in `operationId`.
We will turn off `security` for our endpoint, and add a `Configurator Tools` tag.

We must define a `200` response.
In the response, declare that we are returning an `array` of `PlayingCards`.


Here is what that code looks like:

.. code-block::

    paths:

      ...

      /get_cards:
        parameters:
          - name: cards
            in: query
            required: false
            description: The number of cards to draw. Defaults to one.
            schema:
              type: integer
          - name: decks
            in: query
            required: false
            description: The number of decks to draw from. Defaults to one.
            schema:
              type: integer
        get:
          operationId: crc.api.tools.get_cards
          security: []  # Disable security for this endpoint only.
          summary: Draw cards from a deck of playing cards. For learning purposes only.
          tags:
            - Configurator Tools
          responses:
            '200':
              description: Returns the chosen card(s)
              content:
                application/json:
                  schema:
                    type: array
                    items:
                    $ref: "#/components/schemas/PlayingCards"

We need to define the schema for PlayingCards.

Our playing cards have two properties; suit and value.
They are both strings.

You can add an example which shows up on the API webpage.

Schemas are defined in the `schemas` section.

The schema code looks like this:

.. code-block::

  schemas:

    ...

    PlayingCards:
      properties:
        suit:
          type: string
          example: SPADES
        value:
          type: string
          example: 10

We use the `Connexion <https://connexion.readthedocs.io/en/latest/>`_ framework for our API.

You can check your YML code at https://editor.swagger.io/
