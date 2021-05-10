.. _index-installation:

=======================
Development Environment
=======================

There are two parts to our development environment; CR Connect Workflow, and everything else.

CR Connect Workflow is the Python code we will modify. We need a local copy of it to work with.

Everything else includes CR Connect Front End, CR Connect BPMN, Protocol Builder Mock, and the Database.

We will manage CR Connect Workflow, and use Docker to manage everything else.

----------------------
Stack Deploy Generator
----------------------

Stack Deploy Generator is a Python script `stackdeploy-generator.py` that generates a docker compose file from a template. We use the docker compose file to manage the CR Connect Application Stack.

You can modify the output of Stack Deploy Generator--by passing arguments to the script or editing its default values, to fit your development process.

The arguments are in `args.csv` and the default values are in `defaults.csv`.


For class, we will use the Stack Deploy script to manage everything except CR Connect Workflow, which we will manage ourselves to allow local development.

We will clone the CR Connect Workflow repository locally, and configure the stack deploy environment to work with our local copy of Workflow. Then, we can still use Stack Deploy to manage the rest of the stack.


------------------
Deployment Example
------------------

Here is a step by step process for deploying CR Connect with Stack Deploy.

Working Directory
-----------------

Create a working directory in a suitable location.

.. code-block::

    mkdir development

    cd development

We will use `development` as the top level of our example directory structure.

Clone from github
-----------------

Clone CR Connect Workflow and the Stack Deploy scripts in the development directory.

Directory: development

.. code-block::

    git clone https://github.com/sartography/cr-connect-workflow.git

    git clone https://github.com/sartography/sartography-utils.git

This creates two directories; `development/cr-connect-workflow` and `development/sartography-utils`

CR Connect Workflow
-------------------

First, we will set up CR Connect Workflow

.. code-block::

    cd cr-connect-workflow

    pipenv install --dev

Note:  If you use Visual Studio on Windows, and have trouble installing the python-Levenshtein package,
you might need to download the build tools.
https://visualstudio.microsoft.com/thank-you-downloading-visual-studio/?sku=BuildTools&rel=16

Run Stack Deploy Script
-----------------------

Now, we will work on the deployment script in the development/sartography-utils directory.

Edit the docker-compose defaults in sartography utils

.. code-block::

    cd development/sartography-utils/stackdeploy-generator/cr_connect

Change the PATH_BASE line in defaults.csv to something appropriate.

From

.. code-block::

    "PATH_BASE","$HOME/sartography/docker-volumes/cr-connect/"

To something like

.. code-block::

    "PATH_BASE","/path/to/development/directory/docker-volumes/cr-connect/"

Create a docker-compose file from the sartography utils

.. code-block::

    cd development/sartography-utils/stackdeploy-generator/

    ./stackdeploy-generator.py -F cr_connect -c cr-connect-docker-compose.yml

This creates the file cr-connect-docker-compose.yml and the directory you specified in PATH_BASE, along with a postgres directory in PATH_BASE

Modify Docker Compose File
--------------------------

Now, we need to remove information about the back end from the docker compose file since we are managing it ourselves.

Edit the docker-compose file you just created `cr-connect-docker-compose.yml` and comment out the lines about the backend.


.. code-block::

    #  backend:
    #    container_name: backend
    #    depends_on:
    #       - db
    #       - pb
    #    image: cr-connect-workflow-dev
    #    environment:
    #      - APPLICATION_ROOT=/
    #      - CORS_ALLOW_ORIGINS=localhost:5002,bpmn:5002,localhost:5004,frontend:5004,localhost:4200
    #      - DB_HOST=db
    #      - DB_NAME=crc_dev
    #      - DB_PASSWORD=crc_pass
    #      - DB_PORT=5432
    #      - DB_USER=crc_user
    #      - DEVELOPMENT=true
    #      - LDAP_URL=mock
    ##      - LDAP_URL=ldap.virginia.edu
    #      - PB_BASE_URL=http://pb:5001/v2.0/
    #      - PB_ENABLED=true
    #      - PORT0=5000
    #      - PRODUCTION=false
    ##      - RESET_DB=true
    ##     - ADMIN_UIDS=ajl2j,cah3us,cl3wf # uncomment this to make the default testing user NOT admin
    #      - TESTING=false
    #      - UPGRADE_DB=true
    #    ports:
    #      - "127.0.0.1:5000:5000"
    #    command: ./wait-for-it.sh pb:5001 -t 0 -- ./docker_run.sh

Note that your code may look different from mine.

We also need to comment out 2 lines where bpmn and the front end depend on the backend.

.. code-block::

      bpmn:
        container_name: bpmn
        depends_on:
           - db
    #       - backend
           - pb


.. code-block::

      frontend:
        container_name: frontend
        depends_on:
           - db
    #       - backend
        image: sartography/cr-connect-frontend:dev


Modify CR Connect Workflow
--------------------------

We now need to modify CR Connect Workflow so it talks to the correct ports in the docker container.

The defaults for the docker container are

.. code-block::

    # Backend: 5000
    # Protocol builder : 5001
    # Bpmn: 5002
    # Db: 5003
    # Frontend : 5004

We only need to worry about 5003 for the database and 5004 for the front end. Everything else matches already.

Instance Config
---------------

Flask has a built-in mechanism for modifying your configuration for local development. You can put your modifications into a **config.py** file in the **instance** directory.

Note that you may need to create the instance directory and config.py file.

Flask will read from the config.py file after loading its default configuration. The instance configuration entries will override the default configuration.

.. code-block::

    cd development/cr-connect-workflow

Create the instance directory if it does not already exist.

.. code-block::

    mkdir instance

Change to the instance directory

.. code-block::

    cd instance

Create config.py if it does not already exist.

.. code-block::

    touch config.py

Edit config.py
--------------

These two lines tell the backend that the front end runs on port 5004, and to allow CORS for that port.

.. code-block::

    CORS_ALLOW_ORIGINS = re.split(r',\s*', environ.get('CORS_ALLOW_ORIGINS', default="localhost:4200, localhost:5002, localhost:5004"))
    FRONTEND_AUTH_CALLBACK = environ.get('FRONTEND_AUTH_CALLBACK', default="http://localhost:5004/session")

This tells the back end that the database runs on port 5003, and sets up SQLAlchemy to talk to that port.

.. code-block::

    DB_PORT = 5003
    SQLALCHEMY_DATABASE_URI = environ.get(
        'SQLALCHEMY_DATABASE_URI',
        default="postgresql://%s:%s@%s:%s/%s" % (DB_USER, DB_PASSWORD, DB_HOST, DB_PORT, DB_NAME)
    )

We also need to import the definitions we just used. Add this to the top of config.py

.. code-block::

    import re
    from os import environ
    from config.default import DB_USER, DB_PASSWORD, DB_HOST, DB_NAME


--------------
Start Back End
--------------

Use pipenv to run the CR Connect Workflow Flask application

.. code-block::

    cd ..

    pipenv run python run.py

--------------
Docker Compose
--------------

Use docker-compose to run the rest of the CR Connect application stack.

.. code-block::

    docker-compose -f cr-connect-docker-compose.yml up


--------------
Setup Database
--------------

We need to update the database tables and seed them with some example data.

From the development/cr-connect-workflow directory, run these commands.

.. code-block::

    flask db upgrade

.. code-block::

    flask load-example-data


----
URLs
----

You should now be able to reach these URLs.

`API <http://localhost:5000/v1.0/ui/>`_

`Dashboard <http://localhost:5004/app/home>`_

`Configurator <http://localhost:5002/bpmn/home>`_

`PB Mock <http://localhost:5001>`_

