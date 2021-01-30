# Docker - Selenium Grid with Edge Node

## Building the NodeEdge Image

This container is based of the NodeBase container provided by SeleniumHQ.  

It downloads the latest version of Microsoft Edge for Linux, and the corresponding
Microsoft Edge WebDriver.

To build the NodeEdge Image, issue the following command from the folder containing the NodeEdge DockerFile:
```
docker build -t selenium/node-edge:3.141.59-20210128 . --rm
```

To re-build the image not from cache:
```
docker build -t selenium/node-edge:3.141.59-20210128 . --rm --no-cache
```

## Starting the Grid using Docker Compose

To start the docker compose yaml, i.e. docker-compose.yml, use the following command:

```
docker-compose -f docker-compose.yml up -d
```

To stop the selenium grid via docker compose, issue the command:
```
docker-compose -f docker-compose.yml down
```

## Selenium Grid Console

To view the selenium grid console, visit the following url:

```
http://{your-selenium-hub-url}:grid/console
```