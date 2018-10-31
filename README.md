# dev-setup

## Installation

First build the customized Docker image using:

    docker build --tag custom-odoo:10.0 custom-odoo/

Run Odoo using:

    docker-compose up

## Install extra modules/addons

Load the code of the module into `./addons`

There must be a `__init__.py` and `__manifest__.py` file in the addon folder. Otherwise the addon will not be found by Odoo.

In Odoo in Developer Mode the Apps List must be updated. 

Then the new addon is visible in Odoo and can be installed.

## If you need to access the docker container:

You can see the running instance of Odoo and Postgres docker containers by typing:

    docker ps

You can jump into the Odoo container by executing:

    docker exec -it <containerId> /bin/bash

or

    docker exec -it $(docker ps -aqf "name=dev-setup_web") /bin/bash
