=================
Creating a Script
=================

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
            if len(args) > 0:
                cards = args[0]
            else:
                cards = 1
            if len(args) > 1:
                decks = args[1]
            else:
                decks = 1

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

Best practice for creating a script involves creating a service that does the work, and calling the service from our script.
We cover this in the next section.

