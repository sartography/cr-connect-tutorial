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
