=================
Creating a Script
=================

`Scripts` can be called from a script task in workflows. They are a great way to extend workflow capabilities.

Scripts extend the `Script` base class found in crc.scripts.script, and they must define the
`get_description`, `do_task`, and `do_task_validate_only` methods.

The get_description method should return a string that is displayed when configurators get a list of available scripts.

The do_task_validate_only method is run during the configurator `shield test`.
We only need to make sure that the script *can* run here. We don't want to make any externals calls.

The do_task method is where we put the code we need to achieve our task.

--------------
Example Script
--------------

Create a script that accepts keyword arguments, calls an external api using the arguments, and returns a result.

My data source is a simple API found at https://deckofcardsapi.com that returns cards drawn from one or more decks of playing cards.

My script accepts two keyword arguments; cards and decks.


Example code: crc/scripts/tutorial.py

.. code-block:: Python

    from crc.scripts.script import Script
    import requests


    class TutorialScript(Script):

        def get_description(self):
            return """Simple script for teaching purposes"""

        def do_task_validate_only(self, task, study_id, workflow_id, *args, **kwargs):
            self.do_task(task, study_id, workflow_id, *args, **kwargs)

        def do_task(self, task, study_id, workflow_id, *args, **kwargs):

            drawn_cards = []

            cards = int(kwargs['cards']) if hasattr(kwargs, 'cards') else 1
            decks = int(kwargs['decks']) if hasattr(kwargs, 'decks') else 1

            deck_url = f'https://deckofcardsapi.com/api/deck/new/shuffle/?deck_count={decks}'
            deck_response = requests.get(deck_url)
            deck_id = deck_response.json()['deck_id']

            card_url = f'https://deckofcardsapi.com/api/deck/{deck_id}/draw/?count={cards}'
            card_response = requests.get(card_url)

            for card in range(cards):
                card_value = card_response.json()['cards'][card]['value']
                card_suit = card_response.json()['cards'][card]['suit']
                drawn_cards.append({'suit': card_suit, 'value': card_value})

            return drawn_cards


First, I create an empty list of drawn_cards, and pull cards and decks from the keyword arguments.
Cards and decks both default to 1 if they don't exist as keyword arguments.

I make an API call to get a `deck_id`, and then make another call with the deck_id to get a `card_response`.

From the card_response, I build and return a list of drawn_cards.

We have a script we can call from a workflow. Now we have to build the workflow.

----------------
Example Workflow
----------------

Here is a simple workflow you can use to call this script.

.. image:: /_static/03/01/tutorial_workflow.png

.. image:: /_static/03/01/how_many.png

.. image:: /_static/03/01/get_cards.png

.. image:: /_static/03/01/display_cards.png

And here it is running.

.. image:: /_static/03/01/start_workflow.png

.. image:: /_static/03/01/enter_how_many.png

.. image:: /_static/03/01/display_returned_cards.png

You can find the :download:`bpmn for the workflow <https://github.com/sartography/cr-connect-tutorial/blob/main/_static/tutorial.bpmn>` in the repository.

