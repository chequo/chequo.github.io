# Docker

#### Clear all Docker images and containers and start with clean slate

```cmd
docker system prune -a
```

#### Build docker image (with Docker compose)

```cmd
docker-compose -f docler-compose.yml build
```

#### Launch docker image

```
docker-compose -f docler-compose.yml up
```

#### docker config file

```
version: '2.2'

services:
  
```
