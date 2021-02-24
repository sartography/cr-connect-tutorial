==========
CR Connect
==========

This document serves as an overview for cr-connect development training for UVA. It will focus on cr-connect-workflow.

This documentation is Unix based, but you can develop in a Windows environment

----------
Code Bases
----------

SpiffWorkflow
-------------
https://github.com/sartography/SpiffWorkflow

https://github.com/sartography/SpiffWorkflow.git

Fork of SpiffWorkflow by user knipknap on github. It is a workflow engine implemented in Python.
We only support BPMN.

CR Connect Workflow
-------------------
https://github.com/sartography/cr-connect-workflow

https://github.com/sartography/cr-connect-workflow.git

API that talks to SpiffWorkflow. Written in Python.

CR Connect Frontend
-------------------
https://github.com/sartography/cr-connect-frontend

https://github.com/sartography/cr-connect-frontend.git

Front end for UVA that talks to the API. Written in Angular

CR Connect BPMN
---------------
https://github.com/sartography/cr-connect-bpmn

https://github.com/sartography/cr-connect-bpmn.git

Workflow modeler. Creates BPMN and DMN XML. Can upload supporting documents. Written in Angular

Protocol Builder Mock
---------------------
https://github.com/sartography/protocol-builder-mock

https://github.com/sartography/protocol-builder-mock.git

A mock-up of Protocol Builder. Only meant to be used during development phase. Python and HTML

Sartography Libraries
---------------------
https://github.com/sartography/sartography-libraries
https://github.com/sartography/sartography-libraries.git

Libraries shared by cr-connect-frontend and cr-connect-bpmn. Written in Angular.


-----------------------
Development Environment
-----------------------

Stack Deploy
------------

Stack Deploy is a Python script to bring up multiple code stacks using Docker Compose. You can use it to manage running the different pieces of CR Connect.

It uses docker images stored on

We will use it to manage everything except cr-connect-workflow, which we will manage ourselves to allow local development.

Since we are editing cr-connect-workflow, we will clone that repository locally. Then we will run the stack deploy scripts to manage everything except cr-connect-workflow.

Example
-------

Create a working directory in a suitable location

.. code-block::

    mkdir development

    cd development

Clone CR Connect Workflow and the Stack Deploy scripts

.. code-block::

    git clone https://github.com/sartography/cr-connect-workflow.git

    git clone https://github.com/sartography/sartography-utils.git

Set up CR Connect Workflow

.. code-block::

    cd cr-connect-workflow

    pipenv install --dev

Create a docker image for your local cr-connect-workflow

.. code-block::

    docker build -t cr-connect-workflow-dev .


Edit the docker-compose defaults in sartography utils

.. code-block::

    cd ../sartography-utils/stackdeploy-generator/cr_connect

Change the PATH_BASE line in defaults.csv to something appropriate.

From

.. code-block::

    "PATH_BASE","$HOME/sartography/docker-volumes/cr-connect/"

To something like

.. code-block::

    "PATH_BASE","/path/to/development/directory/above/docker-volumes/cr-connect/"

Create a docker-compose file from the sartography utils

.. code-block::

    cd ..

    ./stackdeploy-generator.py -F cr_connect -c cr-connect-docker-compose.yml

This creates the file cr-connect-docker-compose.yml and the directory you specified in PATH_BASE, along with a postgres directory in PATH_BASE

Edit the docker compose file you created in the line above (cr-connect-docker-compose.yml) and change the **image** line in the backend section to point to the docker image you created for cr-connect-workflow above (cr-connect-workflow-dev).

.. code-block::

      backend:
        container_name: backend
        depends_on:
           - db
           - pb
        image: cr-connect-workflow-dev

Start it all up

.. code-block::

    docker-compose -f cr-connect-docker-compose.yml up



-------------------
CR-Connect-Workflow
-------------------

CR-Connect-Workflow is the API for CR-Connect. It takes requests from the front-end, makes calls to SpiffWorkflow and other aspects of the API, and returns JSON.

API
---

These are the actual API endpoints.

Models
------

Database models. Postgres.

Scripts
-------

These are the scripts that can be called from a workflow. Scripts are the focus of this tutorial.

Services
--------

These are services internal to the API. The API can call these.


-----------------
Creating a Script
-----------------

Example code: crc/scripts/tutorial.py

.. code-block:: Python

    from crc.scripts.script import Script import requests


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


-------------
Writing Tests
-------------

Example code: tests/test_tutorial.py

.. code-block:: Python

    from tests.base_test import BaseTest


    class TestTutorial(BaseTest):

        def test_validate_tutorial(self):
            spec_model = self.load_test_spec('tutorial')
            response = self.app.get('/v1.0/workflow-specification/%s/validate' % spec_model.id, headers=self.logged_in_headers())
            self.assert_success(response)

        def test_draw_cards(self):

            workflow = self.create_workflow('tutorial')
            workflow_api = self.get_workflow_api(workflow)

            first_task = workflow_api.next_task
            self.assertEqual('Task_Hello', first_task.name)

            result = self.complete_form(workflow_api, first_task, {'decks': 1, 'cards': 2})
            self.assertEqual(2, len(result.next_task.data['drawn_cards']))

            card_1 = f'{result.next_task.data["drawn_cards"][0]["value"]} of {result.next_task.data["drawn_cards"][0]["suit"]}'
            card_2 = f'{result.next_task.data["drawn_cards"][1]["value"]} of {result.next_task.data["drawn_cards"][1]["suit"]}'
            self.assertEqual(f'</H1>Good Bye</H1>\n\n<div><span>{card_1}</span></div>\n\n<div><span>{card_2}</span></div>\n', result.next_task.documentation)
