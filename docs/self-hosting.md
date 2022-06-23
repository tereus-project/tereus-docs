---
sidebar_position: 5
---

# Self hosting Tereus

Tereus is open-source, which means you can host it yourself. The source code is available on [Github](https://github.com/tereus-project).

## Requirements

- Docker
- Docker Compose
- Git
- Node.js v16.x

## Setting up the API and storage services

The API is written in Go, and is available on [Github](git@github.com:tereus-project/tereus-api).

The `docker-compose.yml` file inside the repo contains the following services:

- `api`: The API service
- `postgres`: The PostgreSQL service for database
- `minio`: The MinIO service for object storage
- `nsqd`: The NSQ service for queuing

Setting up the API with the services is done by running the following commands:

```sh
git clone git@github.com:tereus-project/tereus-api.git
cd tereus-api
cp env.example .env
docker-compose up -d
```

The docker-compose services, including the API, use environment variables to configure themselves. The `.env.example` file contains the environment variables that you need to set for a local environment, but you can modify the file to your own needs.

## Setting up the web interface

The web interface is written in React with [Remix](https://remix.run/), and is available on [Github](https://github.com/tereus-project/tereus-front).

You can setup the frontend by running the following commands:

```sh
git clone git@github.com:tereus-project/tereus-front.git
cd tereus-front
cp env.example .env
npm install
npm run dev
```

## Setting up a transpiler

Tereus uses transpiler services to transpile the code. For example, we have a dedicated transpiler service for C-to-Go, which is available on [Github](https://github.com/tereus-project/tereus-transpiler-c-go)

The transpiler uses the NSQ message queue to get the transpilation submissions and to return the status, so you will need to setup the API beforehand as it relies on the same Docker network.

You can run the transpiler with:

```sh
git clone git@github.com:tereus-project/tereus-transpiler-c-go.git
cd tereus-transpiler-c-go
cp env.example .env
docker-compose up -d
```
