---
layout: post
title: "Learn MSSQL and PostgreSQL with Docker"
date: "2022-02-01"
description: "In this tutorial, we use Docker to spin up database containers for us to practice our enumeration skills. At the end of this tutorial you will have run MSSQL Server 2019 from Microsoft and PostgreSQL inside docker container and enumerated them with `sqlcmd` and `psql` commands."
coverimage: Learn_MSSQL_and_PostgreSQL_with_Docker.jpg
tags: database docker mssql postgresql psql sqlcmd
published: true
posttype: article
categories: tutorial
---

In this tutorial, we will be using Docker to spin up database containers for us to enumerate. There are many articles on setting up and installing docker on your favourite platform so this tutorial will start from the point where docker is already installed. 

## Installing Databases

For this tutorial, we will be using Docker containers for MSSQL Server 2019 and PostgreSQL 14.

The official docker hub page for Microsoft MSSQL Server 2019 and PostgreSQL are located at the following URL. 

* https://hub.docker.com/_/microsoft-mssql-server
* https://hub.docker.com/_/postgres/

Before we start configuring our containers we should touch on some basics.  If we use the `docker pull` command we can pull a Docker image from the Docker hub to our machine. Pulling an image just gives us access to that image for when we need it. If however, we use the command `docker run` it will check if the image has already been pulled and if it hasn't been pulled previously this will pull the image and then run it

Most docker installation guides take the user through the process of installing Docker and then running a "Hello World" example. The following example is the Docker equivalent of a programmers "Hello World" script

```
$ docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
1b930d010525: Pull complete 
Digest: sha256:6f744a2005b12a704d2608d8070a494ad1145636eeb74a570c56b94d94ccdbfc
Status: Downloaded newer image for hello-world:latest
```

We can see that when we used the `docker run` command that Docker was "Unable to find 'hello-world:latest' locally" so it made a pull. 

## Microsoft SQL Server

Running MSSQL Server 2019
```
docker run --name sqlserver2019 -e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=Password123' -p 1433:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```

Compared to the simple "Hello World" example we just looked at we can see the MSSQL Database requires a few more command-line arguments. 


docker run --name sqlserver2019-2 -e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=Password123' -p 2433:1433 -d mcr.microsoft.com/mssql/server:2019-latest

## PostgreSQL


docker run --name postgresql -p 5432:5432 -e POSTGRES_PASSWORD=Password123 -d postgres:latest
docker run --name postgresql -p 6432:5432 -e POSTGRES_PASSWORD=Password123 -d postgres:latest


## Interacting with Microsoft SQL Server

Log into sqlserver2019 docker container
```
docker exec -it sqlserver2019 bash
```

View OS information
```
cat /etc/os-release
```

Log into SQL Server using SQLCMD command line tool
```
/opt/mssql-tools/bin/sqlcmd -U sa -P Password123
```

View version information
```
SELECT @@Version;
GO
```

List databases currently on the server
```
SELECT name FROM sys.databases;
GO
```

Create a new database
```
CREATE DATABASE evilsaint;
GO
```

Quit SQLCMD CLI tool
```
exit
```

Exit out of the docker container
```
exit
```

## PostgreSQL

Log into postgresql docker container
```
docker exec -it postgresql bash
```

View OS information
```
cat /etc/os-release
```

Log into PosgreSQL server using PSQL command line tool
```
psql -U postgres
```

View help documentation
```
help
```

List databases currently on the server
```
\l
```

Create a new database
```
CREATE DATABASE evilsaint;
```

Quit PSQL CLI tool
```
\q
```

Exit out of the docker container
```
exit
```

Run postgresql database 2 for bruteforce
```
docker exec postgresql-2 bash
```



## Docker Commands

view all running containers
```
docker ps
```

view all containers regardless of status
```
docker ps -a
```

stop a container
```
docker stop sqlserver2019
```

start a container
```
docker start sqlserver2019
```

remove a container
```
docker rm sqlserver2019
```