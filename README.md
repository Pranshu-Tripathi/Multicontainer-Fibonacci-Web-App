# Multicontainer-Fibonacci-Web-App


# Purpose
This is a hands on learning with deployment and development industry-grade standard process, that we follow in todays web application development.
This is not very development centric application. Rather we are learning how to use Docker, Travis CI and AWS to cleanly deploy and test our application progressively and smoothly.

# Application Functionality

- This is a fibonacci calculation application for any index.
- The application provides a form to user asking for the index for which they want to know the fibonacci number.
- The application shows a home page where it shows list of all the calculated fibonacci index and value pairs.
- It also shows the list of all the indexes that user has tried for in this application.

# Development phase

Lets get an idea on what containers are we deploying.

## Containers that run on your system

- Nginx: The routing service that will redirect api requests to server and other to client.
- Client: The react application that shows the UI centric components to user.
- Server: An express server, that will act as an API end point to our client.
- Worker: Actual computational component, that subscribes to redis container and calculates values for a given input. (It is intentionally made slow).
- Postgres: A database that stores the list of all the indexes that user has asked for.
- Redis: A different database that stores the key value pair for each input.

## Travis CI

We used this open source tool to have a smooth CI/CD attached to any changes we make in this repository.
Whenever we push any changes travis does the following:
- Build all the containers and run tests.
- If tests succeed it generates production grade build images and saves them on docker hub.

# Production Phase

## Changes in containers:
- Instead of running our own postgres and redis containers we use the AWS Elastic Cache (Redis) and AWS Elastic RDS(Postgres).

## Travis CI

With an IAM User credentials, when changes are pushed to main branch, Travis starts the deployment on AWS with the new docker hub images.

## AWS Elasticbeanstalk

We use this to deploy our multi-container application. We set custom security groups on these different services.
EBS will read the docker-compose.yml file and configure all the containers.
