# Insight DevOps Engineering Systems Puzzle -- Solution By Vamshi Reddy

## Table of Contents
1. [Puzzle Introduction](README.md#Puzzle-Introduction)
2. [Puzzle details](README.md#puzzle-details)
3. [List of Bugs found and fixed](README.md#List-of-Bugs-found-and-fixed)

# Puzzle Introduction

Imagine you're on an engineering team that is building an eCommerce site where users can buy and sell items (similar to Etsy or eBay). One of the developers on your team has put together a very simple prototype for a system that writes and reads to a database. The developer is using Postgres for the backend database, the Python Flask framework as an application server, and nginx as a web server. All of this is developed with the Docker Engine, and put together with Docker Compose.

Unfortunately, the developer is new to many of these tools, and is having a number of issues. The developer needs your help debugging the system and getting it to work properly.

# Puzzle details

The codebase included in this repo is nearly functional, but has a few bugs that are preventing it from working properly. The goal of this puzzle is to find these bugs and fix them. To do this, you'll have to familiarize yourself with the various technologies (Docker, nginx, Flask, and Postgres). You definitely don't have to be an expert on these, but you should know them well enough to understand what the problem is.

Assuming you have the Docker Engine and Docker Compose already installed, the developer said that the steps for running the system is to open a terminal, `cd` into this repo, and then enter these two commands:

    docker-compose up -d db
    docker-compose run --rm flaskapp /bin/bash -c "cd /opt/services/flaskapp/src && python -c  'import database; database.init_db()'"

This "bootstraps" the PostgreSQL database with the correct tables. After that you can run the whole system with:

    docker-compose up -d

At that point, the web application should be visible by going to `localhost:8080` in a web browser. 

## List of Bugs found and fixed
* set database data(dbdata) directory relative to the docker-compose.yml file.
    volumes:
      - "./dbdata:/var/lib/postgresql/data" 
* expose container port 80 on host port 8080 (port binding 8080:80)
* Update Item model class with Printable representation of Postgres object.
* Nginx proxy header should have only one Host. 
  Inside conf.d/flaskapp.conf file, developer has set 2 host header. 
  proxy_set_header Host $http_host;
  proxy_set_header Host $host; 
  Due to this while redirecting from "/" home page to "/success" success page, we have seeig multiple localhost's in the url "localhost,localhost:8080/success". To solve this I have removed proxy_set_header Host $host; line from Nginx config file.
