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
 * [Performance tips for MacOS](#performance-tips-for-macos)
   * [Cache directory](#cache-directory)
   * [OpCache](#opcache)   
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
 4. Make sure, that `APP_PATH` directory is empty.
 5. Execute `./console create-project symfony/website-skeleton . '4.*'` to install project using composer`s create-project 
    command. You can use different package name and version.

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

## Performance tips for MacOS
### Cache directory
Shared volumes, even with NFS can be slow so its good idea to move as much data as possible outside shared directories.
The easiest way is to move cache to the `/tmp` directory. To do it in Symfony you need to override the
`Kernel::getCacheDir` method in your `Kernel.php` file: 
```php
<?php
// ...    
class Kernel extends BaseKernel {
    // ...
    public function getCacheDir()
    {
        return '/tmp/symfony/'.$this->environment;
    }
    // ...
}
```
### OpCache
Another way to reduce filesystem calls is to use the OpCache extension. To install this extension, add to the 
`./docker/php/Dockerfile` file following lines:
```dockerfile
RUN    docker-php-ext-install \
        opcache \
    && docker-php-ext-configure \
        opcache \
            --enable-opcache \
    && docker-php-ext-enable \
        opcache \
    && docker-pcs-php-ext-config opcache \
        # Fine tune below configuration to your needs:
        opcache.memory_consumption=256 \
        opcache.interned_strings_buffer=8 \
        opcache.max_accelerated_files=1000000 \
        opcache.fast_shutdown=1 \
        opcache.enable_cli=1 \
        opcache.enable=1 \
        # If your app makes a lot of ajax request, it is good idea to set revalidate_freq to 2 or 3 seconds
        opcache.revalidate_freq=0
```
If you already built container, you need to rebuild it using `./console compose build php` command and run it again.

## Usage
 * `./console start [SERVICES...]` - starts all or selected Docker containers
 * `./console stop [SERVICES...]` - stops all or selected Docker containers
 * `./console compose [ARGS...]` - wrapper for docker-compose commands
 * `./console install` - installs application
 * `./console create-project PACKAGE_NAME [VERSION]` Creates project using composer command
 * `./console shell [--user USER] [--shell SHELL] SERVICE` - opens container's shell
 * `./console clean` - removes all images, containers and volumes associated with your project
