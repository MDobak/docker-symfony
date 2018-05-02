# Docker + NFS + Symfony
Sample Docker configuration for Symfony 3 and Symfony 4 projects for Linux and Mac OS machines with NFS
based volumes for Mac OS.

## Contents
 * [Requirements](#requirements)
 * [Installation](#installation)
   * [New project](#new-project)
   * [Existing project](#existing-project)
 * [Usage](#usage)

## Requirements
 * Docker Engine 1.13.0 or never
 * Docker Compose 1.10.0 or never
 * Docker For Mac 18.03.0-ce-mac60 or never (for Mac OS only)
 * El Capitan 10.11 and newer (for Mac OS only)

## Installation
Depending on your operating system, create `.env` file based on `.env-linux.dev` or `.env-macos.dev` file.
Change `PROJECT_NAME` variable if you working on multiple projects to avoid name collisions. You can also
change other variables depending on your need. On Linux it is important to set user and group ID
in `APP_USER_ID` and `APP_GROUP_ID` variables to the same IDs as user you working on.

### New project
If you creating new project, use `./console create-project symfony/symfony .` command.

### Existing project
Copy all project files to desired location and set correct paths in `.env` file. By default all project files
should be places in the `/app` directory.

## Usage
 * `./console start` - starts Docker container
 * `./console stop` - stops Docker container
 * `./console compose` - wrapper for docker-compose commands
 * `./console bash [--user USER] [--app APP] [--shell SHELL]` - opens container's shell
 * `./console clean` - removes all images, containers and volumes associated with your project
