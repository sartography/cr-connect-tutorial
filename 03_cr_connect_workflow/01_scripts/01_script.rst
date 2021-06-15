==============
Example Script
==============

----
Goal
----

Create a script that accepts keyword arguments, calls an external api using the arguments, and returns a result.

-----
Setup
-----

My data source is a simple API found at https://deckofcardsapi.com that returns cards drawn from one or more decks of playing cards.

My script accepts two keyword arguments; cards and decks.

-------
Example
-------

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

-----------
Explanation
-----------

First, I create a class that extends the Script class in crc.scripts.script.

The **get_description** method returns a string describing the script. This is for configurators.

The **do_task_validate_only** method calls do_task.

In **do_task** we have the actual code that runs when our script is called.

The do_task method accepts two keyword arguments; cards and decks.

We use those arguments to call the remote API, parse the response, and return a list of cards.

----
Next
----

Now that we have a script we can call from a workflow, we have to build the workflow.
