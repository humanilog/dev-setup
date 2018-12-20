# dev-setup

## URL / Port for 10.0
    
    localhost:8070 or 127.0.0.1:8070

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

## wait-for-it script

The wait-for-it bash script is used to start the odoo docker image only after the postgres server is up. Check out **Troubleshooting** to understand why. The script is cpoied from https://github.com/vishnubob/wait-for-it.

## Troubleshooting 

### print pdf with the right styles and images
When odoo is started in docker, odoo takes on `http://localho....` as the global setting for its own network addres. However, docker puts a layer on top and changes the network behavior.
If you are then printing a report as pdf, the html to pdf converter `wkhtmltopdf` does not find the right styles and images referenced in the html anymore.
To solve this problem you have to update the setting 

    web.base.url

You can find this setting in `debug` mode in 

    Settings > Technical > Parameters > System Parameters

#### How do you find out the address to put in the setting?

1. Find the docker network container for your odoo with

        docker network ls
    
    There should be an entry like `<name of your local dev-setup repository>_default`.
    ![alt text](https://github.com/humanilog/dev-setup/blob/10.0/readme_pics/step_1.png)


2. Copy the `NETWORK ID` of that container!

3. Run the command

        docker network inspect <NETWORK ID>
        
   ![alt text](https://github.com/humanilog/dev-setup/blob/10.0/readme_pics/step_3.png)

4. In the key `Containers` you will find two docker containers. Copy the `IPv4Address` of the `odoo` container (NOT the postgres one). Put this address into the `web.base.url` setting in the format 

        http://<ip-address>:8069

### cannot connect to postgres during installation
It happens - especially during the first installation - that the postgres db is not fast enough in starting up. Odoo is then already trying to connect to postgres and then exits with an error code.

Please wait until postgres is up before you kill docker.

Depending on the os it then might just work if you try it again.
It happens that postgres or odoo saves a weird error because of the first error exit. Then you have to delete all data of odoo and postgres before starting it up again.

Therefore try

    docker rm $(docker ps -a -q)	

    docker volume prune

and if it is still not working also clear all volumes folders of dev-setup.
