# dev-setup

Run Odoo using:

    docker-compose up

Now you can see the running instance of the Odoo and Postgres instance by typing:

    docker ps

You can jump into the Odoo container by executing:

    docker exec -it <containerId> /bin/bash

or

    docker exec -it $(docker ps -aqf "name=dev-setup_web") /bin/bash
