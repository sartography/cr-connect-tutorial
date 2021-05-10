==================
Building a Service
==================

Services are internal code related to a specific function. We have services that manage users and studies,
interact with Protocol Builder and the LDAP server, send emails, and make calls to SpiffWorkflow.

Services should not be called directly from outside the system. They should be called by scripts, the API, and other services.

----------------
Tutorial Service
----------------

Let's build a service that replaces the do_task method in our script. We can then call the service from our script.

.. code-block:: Python

    import requests


    class TutorialService(object):

        @staticmethod
        def pick_a_card(cards, decks):
            drawn_cards = []

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

We can now modify our script to call this new service.

.. code-block:: Python

    from crc.scripts.script import Script
    from crc.services.tutorial_service import TutorialService

    class TutorialScript(Script):

        def get_description(self):
            return """Simple script for teaching purposes"""

        def do_task_validate_only(self, task, study_id, workflow_id, *args, **kwargs):
            self.do_task(task, study_id, workflow_id, *args, **kwargs)

        def do_task(self, task, study_id, workflow_id, *args, **kwargs):

            cards = kwargs['cards']
            decks = kwargs['decks']

            drawn_cards = TutorialService.pick_a_card(cards=cards, decks=decks)
            return drawn_cards
