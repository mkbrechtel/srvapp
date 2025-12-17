# srvapp — serve applications from /srv/{app_hostname}/

We store our application deployments in /srv/{app_hostname}

With this ansible collection we provide a standarized way to handle such application deployments.

Each /srv/{app_hostname} has it's own README.app.md

We provide a central reverse proxy setup to expose those apps for users.

For certain components like databases we provide a default deployment option.
