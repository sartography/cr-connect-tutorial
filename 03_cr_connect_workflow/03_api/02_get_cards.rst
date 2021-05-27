=======================
Get Cards
=======================

We defined an API endpoint. Now, we need to write the Python code to support it.

When we defined the get_cards endpoint, we set `operationId` to `crc.api.tools.get_cards`.

.. code-block::

        get:
          operationId: crc.api.tools.get_cards

This means that the API expects a method called `get_cards` in the `crc.api.tools` module.

We need to add this method.

---------
get_cards
---------

We have already done the heavy lifting, so we don't have to write much Python code.

get_cards has to:

- take in two parameters; cards and decks,
- make a call to our service, and
- return a list of playing cards.

.. code-block::

    def get_cards(cards=1, decks=1):
        drawn_cards = TutorialService.pick_a_card(cards=cards, decks=decks)
        return drawn_cards

Remember that our API definition says that our parameters are not required.
So, we had to give them default values in our method definition.
