# Docker + Symfony
Docker boilerplate configuration for Symfony projects with fast NFS volumes on MacOS.

## Features
 * Supports Linux and MacOS
 * Automatically configures NFS exports on MacOS if needed
 * No other dependencies required than Docker
 * Requires no configuration for developers
 * Easy to use for team members not familiar with Docker 
 * Docker images contains cron, composer and supervisord (check out [https://github.com/MDobak/php-common-stack](https://github.com/MDobak/php-common-stack) for more)

## Contents
 * [Requirements](#requirements)
 * [Installation](#installation)
   * [Creating new project](#creating-new-project)
   * [Migrating existing project](#migrating-existing-project)
 * [File permissions on MacOS](#file-permissions-on-macos)  
 * [Usage](#usage)

## Requirements
 * Docker Engine 1.13.0+
 * Docker Compose 1.10.0+
 * Docker For Mac 18.03.0-ce-mac60+ (_for Mac OS only_)
 * El Capitan 10.11+ (_for Mac OS only_)

## Installation
### Creating new project
 1. Change the `PROJECT_NAME` variable in both `.env-linux.dev` and `.env-macos.dev` files if you are working on multiple 
    projects to avoid name collisions. You can also change other variables depending on your needs.
 2. Set correct paths in `.env-*.dev` files if you want to use different location than the `/app` directory.
 3. Start **only** `php` container using `./console start php` command.
 4. Make sure, that `APP_PATH` directory exist and it's empty.
 5. Log in into container's shell using `./console shell php` command.
 6. Use composer to create new symfony project: `composer create-project symfony/symfony .`.
 7. Log out from container with `exit` command.
 8. Use `./console start` command to run other containers.

### Migrating existing project
 1. Change the `PROJECT_NAME` variable in both `.env-linux.dev` and `.env-macos.dev` files if you are working on multiple 
    projects to avoid name collisions. You can also change other variables depending on your needs.
 2. Copy all project files to desired location. By default all project files should be placed in the `/app` directory. 
 3. Set correct paths in `.env-*.dev` files if you want to use different location than the `/app` directory.
 4. Start containers using `./console start` command.
 5. If necessary, you can log in into container's shell using `./console shell php` command.

## File permissions on MacOS
On MacOS Docker uses NFS for volume mounting. By default NFS exports are mounted with the UID and GID of the currently 
running user on the Mac and it is not possible to change permissions and ownership of these files. Although, despite of
UID and GID your containers will have access to these files.

## Usage
 * `./console start [SERVICES...]` - starts all or selected Docker containers
 * `./console stop [SERVICES...]` - stops all or selected Docker containers
 * `./console compose [ARGS...]` - wrapper for docker-compose commands
 * `./console install` - installs application
 * `./console shell [--user USER] [--shell SHELL] SERVICE` - opens container's shell
 * `./console clean` - removes all images, containers and volumes associated with your project
