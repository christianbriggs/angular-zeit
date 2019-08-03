# asyde
One-on-one discussion for Twitter

# Dev Prerequisites
* (required) Install and run [Docker Desktop for Mac](https://www.docker.com/products/docker-desktop)
* (recommended but not necessar) Install [Kitematic](https://kitematic.com/docs/) (visual tool for managing Docker)
* (required) Install Git. Pretty sure it is standard on OSX.
* (recommended) [Github Desktop](https://desktop.github.com/) or other git client

# Worflow
This workflow uses Docker-Compose to orchestrate processes

## Development

### Start Docker
Make sure Docker is running and Docker-Compose is installed. To check, you should be able to type `docker` and `docker-compose` in your terminal and get a list of available commands. 

**Note:** All of the following commands should be done in the terminal at the root of this project directory

### Build the Image
In the root of this project directory, fire up a terminal and type

`docker-compose up -d --build`

If successful, you should be able to see the site running at [http://localhost:4201](http://localhost:4201)

To double-check, you can open Kitematic and see the currently running Docker containers, which should now include the "asyde_dev" container. 

### Run Unit and E2E Tests
With the application running, in the terminal type `docker-compose exec asyde_dev ng test --watch=false` to run unit tests, or `docker-compose exec asyde_dev ng e2e --port 4202` to run End-to-End (E2E) tests.

### Stop the Container
`docker-compose stop`

## Production

### Build the Production Image
`docker-compose -f docker-compose-prod.yml up -d --build`

### Run Unit and E2E Tests
`docker-compose exec asyde_prod ng test --watch=false`

`docker-compose exec asyde_prod ng e2e --port 4202`

### Stop the Production Container
`docker-compose stop`



# Alternate Docker-Only Workflow (not recommended)

## Development

### Build and Tag the Docker Image
`docker build -t asyde:dev .`

### Run the Docker Container Normally
`docker run -v ${PWD}:/app -v /app/node_modules -p 4201:4200 --rm asyde:dev`

### Run the Docker Container in the Background
`docker run -d -v ${PWD}:/app -v /app/node_modules -p 4201:4200 --name CONTAINERNAME --rm asyde:dev`

### Stop  the Docker Container
`docker stop CONTAINERNAME`

### Run Unit and E2E Tests

`docker exec -it CONTAINERNAME ng test --watch=false`

`docker exec -it CONTAINERNAME ng e2e --port 4202`

## Production

### Build and Tag the Production Docker Image
`docker build -f Dockerfile-prod -t asyde:prod .`

### Run the Production Docker Container Normally
`docker run -it -p 80:80 --rm asyde:prod`

### Stop the Production Docker Container
`docker stop CONTAINERNAME`

### Run Unit and E2E Tests

`docker exec -it CONTAINERNAME ng test --watch=false`

`docker exec -it CONTAINERNAME ng e2e --port 4202`
