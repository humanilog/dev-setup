# dev-setup

## Default

### URL / Port for 9.0
    
    localhost:8071 or 127.0.0.1:8071


## Installation

Run Odoo using:

    docker-compose up

The customized odoo image `custom-odoo` is build within. In case it is not run:

    docker build --tag custom-odoo:11.0 custom-odoo/

## Install extra modules/addons

Load the code of the module into `./addons`

There must be a `__init__.py` and `__manifest__.py` file in the addon folder. Otherwise the addon will not be found by Odoo.

In Odoo in Developer Mode the Apps List must be updated. 

Then the new addon is visible in Odoo and can be installed.

## If you need to access the docker container:

You can see all instance of Odoo and Postgres docker containers by typing:

    docker ps -a

You can jump into the Odoo container by executing:

    docker exec -it <container> /bin/bash
    
## Networking

If you want to open up your local odoo server in your local network, read this section.

Run `ipconfig`(Windows) or `ifconfig`(Linux) to find out your ip address in your local network. To access your server from another computer type in your browser

    <your_ip_address>:8071

For the docker ip adresses run

    docker inspect -f '{{.Name}} - {{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' $(docker ps -aq)
    
### Windows

In Windows docker is run in a virtual machine. If you cannot access your server from outside you have to forward the port of the VM. The docker default network address is `10.0.75.1`. Please change it in the command below if you adjusted this setting in docker for windows.

    netsh interface portproxy add v4tov4 listenport=8069 listenaddress=10.0.75.1 connectport=8069 connectaddress=<your ip address>

It should then work as described above.

An other problem could also be the Windows firewall.
    
## Troubleshooting

### HOWTO delete docker containers

If you are stuck with an error, this is often the fastest solution.

Choose the containers you want to delete from 

    docker ps -a

Delete them via 

    docker rm <container>
    docker volume prune

To delete all do 

    docker rm $(docker ps -a -q)	
    docker volume prune

### Postgres slower than odoo when starting up

It happens - especially during the first installation - that the postgres db is not fast enough in starting up. Odoo is then already trying to connect to postgres and then exits with an error code.

In such a case, please wait until postgres is up before you kill docker.

Therefore, we added the following workaround:

#### wait-for-it script

The wait-for-it bash script is used to start the odoo docker image only after the postgres server is up. The script is copied from https://github.com/vishnubob/wait-for-it.

### Python ImportERROR: No import module <module_name> or similar

If you start up odoo and you have an incorrect python file in a module in your addons folder, it can lead to this error. Odoo preloads all python files in the addons folder even if the corresponding modules are not installed.

To solve this issue:

    Clear your Browser cache!
    Either fix the python error or remove the module from the addons folder.

### Windows

#### ERROR: Cannot start service odoo: driver failed programming external connectivity on endpoint ...

This error can appear if you are not shutting down the server correctly, e.g. power turned off or you just shut the shell without shutting down the server.
Mostly, it is enough to 

    Restart your docker server
    
Check out this [link](https://github.com/docker/for-win/issues/1038) for this problem!
