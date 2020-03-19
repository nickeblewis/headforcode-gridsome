---
title: Checking out Hasura!
tags: docker, hasura, docker-compose, devops, graphql
excerpt: Hasura is an open source engine that connects to your databases & microservices and auto-generates a production-ready GraphQL backend. 
createdAt: 2020-03-01 11:26:00
---
## Introduction

Hasura is an open source engine that connects to your databases & microservices and auto-generates a production-ready GraphQL backend. 

This article is a quick walk through of how you can use Hasura locally with the aid of Docker and Docker Compose. So we are hoping that you've got these installed already ;-) See links below if not:

- [Docker](https://docs.docker.com/install/)
- [Docker Compose](https://docs.docker.com/compose/install/)

## Step 1: Get the docker-compose file

You need to create a new folder first, so here is what I did:

```bash
cd proj
mkdir hasura-docker-test && cd $_
```

``proj`` is where I keep all of my code projects but it can of course be anything, however you choose to set out your folder structures.

Next you need to grab the remote file(s):

```bash
wget https://raw.githubusercontent.com/hasura/graphql-engine/stable/install-manifests/docker-compose/docker-compose.yaml
```

Just do a quick ``ls`` and you should see the addition of a ``docker-compose.yaml`` in your folder.

## Step 2: Run Hasura GraphQL engine and Postgres

```bash
$ docker-compose up -d
```

Which runs the docker container in a detached state and the next step is to take a look at the containers' status:

```bash
$ docker ps
```

## Step 3: Open up the Hasura Console

Open ``http://localhost:8080/console`` in your browser and you will see an instance of the Graphiql UI ready to go!
