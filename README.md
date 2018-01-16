# This repo contains docker files for development environment in Le Filament

[![](https://img.shields.io/badge/licence-AGPL--3-blue.svg)](http://www.gnu.org/licenses/agpl "License: AGPL-3")

Currently there are 2 dockers created for which we only provide here example docker-compose to be modified according to your needs: 

## Ansible
Docker to be used to run Ansible commands, it will automatically download an Ansible repo from variable ` GIT_REPO ` and make an image out of it.
It will also set the default user.mail for git to ` GIT_EMAIL `

Basically, the Dockerfile is expecting you to provide the following file structure at the same place where you have the DockerFile:
- ssh / id_rsa (private key for connecting to repo, will be replaced afterwards by the one in Ansible Vault wihch needs to be defined as ` ansible_depl_ssh_key `)
- .vault.txt (Ansible vault password)


You can then change directory to ansible, and launch the following command to build the image out of the DockerFile:
```bash 
docker build --force-rm -t ansible .
```

You can then either run a command directly from the command line:
```bash
docker run -it --rm ansible ansible-playbook -i inventory.inv playbook.yml -l Server2 -t init -CD
```

Or you can enter the container to run your commands as usually and push commits in case you need to make changes (make sure you use a proper read-write password for BitBucket in that later case):
```bash
docker run -it --rm ansible sh
```

### docker-compose
An example docker-compose file is present in the repo, where you can set both variables listed above, and also mount /root as a volume in order to keep the modifications you made and not yet uploaded to origin, but also history of commands.
The easiest way to start from that file is to copy it in docker-compose.yml and run for instance one of the following command:
```bash
docker-compose run --rm ansible sh
docker-compose run --rm ansible ansible-playbook -i inventory.inv playbook.yml -l Server2 -t init -CD
docker-compose run --rm ansible git pull --rebase
```

## Odoo (odoo-dev directory)
Reuse code from [Tecnativa](https://github.com/Tecnativa/docker-odoo-base#scaffolding)

There are different services in this directory:

- setup-devel.yaml : docker-compose file to download from git all files in odoo-dev/odoo/custom/src/repos.yaml (for exact syntax see the explanations from Tecnativa)
- docker-compose-example.yaml : docker-compose file to run Odoo daemon on the local machine and expose port 8069, this service will mount Odoo code and addons from local directory odoo-dev/odoo/custom/src/ and links from odoo-dev/odoo/auto/addons/ (which can be populated by using the setup-devel compose file or manually).
- backups/ in this directory a docker-compose file allows to restore the local odoo database from a duplicity backup placed in the odoo-dev/backups/odoo directory (for instance in order to retrieve the production database)

Only example docker-compose files are present in this repo and need to be amended with your proper environment variables (such as users, passwords, etc.). The name of the variables in each docker-compose file should be enough to understand what it represents, so these are not described here.

# Credits

## Contributors

* Remi Cazenave <remi-filament>


## Maintainer

[![](https://le-filament.com/img/logo-lefilament.png)](https://le-filament.com "Le Filament")

This module is maintained by Le Filament
