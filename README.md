# This repo contains docker files for Odoo development environment in Le Filament

Based on scaffolding Doodba code from [Tecnativa](https://github.com/Tecnativa/doodba-scaffolding)

There are differents services in this directory:

- setup-devel.yml : docker-compose file to download from git all files in odoo-dev/odoo/custom/src/repos.yaml (for exact syntax see the explanations from Tecnativa)
- docker-compose.yml : docker-compose file to run Odoo daemon on the local machine and expose port 8069, this service will mount Odoo code and addons from local directory odoo-dev/odoo/custom/src/ and links from odoo-dev/odoo/auto/addons/ (which can be populated by using the setup-devel compose file or manually).

You may need to adapt variables in docker-compose files, these are either considered as self-explained by their names or some information may be found in [Tecnativa Doodba documentation](https://github.com/Tecnativa/doodba/blob/master/README.md).

In order to start using the docker afterwards: 

```bash
docker-compose build --pull
docker-compose -f setup-devel.yaml run --rm odoo
docker-compose up
```


# Credits

## Contributors

* Remi Cazenave <remi-filament>


## Maintainer

[![](https://le-filament.com/img/logo-lefilament.png)](https://le-filament.com "Le Filament")

This module is maintained by Le Filament
