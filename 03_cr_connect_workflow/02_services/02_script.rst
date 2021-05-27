=============
Modify Script
=============

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
