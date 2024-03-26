# Dockerizing the application

Part of the process of deploying an application is to containerize it. This involves creating a Docker image that contains the application and its dependencies. This image can then be built and run in a Docker container.


## Dockerfiles

Dockerfiles are used to define the steps needed to create a Docker image. They specify the base image to use, any dependencies to install, and the commands to run to set up the application. For my application, I created a Dockerfile for each service.

The services for which I created Dockerfiles are:

{type="medium"}
Frontend
: The frontend service is a React application that serves the user interface for the application.

API Gateway
: The API Gateway service is an ASP.Net Core application that acts as a reverse proxy for the other services.

Post Service
: The Post Service is an ASP.Net Core application that provides an API for creating and retrieving posts.

## Creating Dockerfiles

### Frontend Dockerfile

The frontend service is a React application that serves the user interface for the application. The Dockerfile for the frontend service might look something like this:

```Dockerfile
FROM node:21-alpine AS build

WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

RUN npm run build

FROM nginx:alpine

COPY ./.nginx/nginx.conf /etc/nginx/nginx.conf

COPY --from=build /app/dist /usr/share/nginx/html

EXPOSE 3000

CMD ["nginx", "-g", "daemon off;"]
```

This Dockerfile uses a multi-stage build to first build the React application and then copy the built files into an Nginx container to serve them. It sets up Nginx to serve the files and exposes port 3000.

### API Gateway Dockerfile

The API Gateway service is an ASP.Net Core application that acts as a reverse proxy for the other services. The Dockerfile for the API Gateway service looks like this:

```Dockerfile
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER app
WORKDIR /app
EXPOSE 8080

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["API-Gateway/API-Gateway.csproj", "API-Gateway/"]
RUN dotnet restore "./API-Gateway/API-Gateway.csproj"
COPY . .
WORKDIR "/src/API-Gateway"
RUN dotnet build "./API-Gateway.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./API-Gateway.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "API-Gateway.dll"]
```

This Dockerfile sets up the API Gateway service to run on port 8080 and copies the built application into the container.

### Post Service Dockerfile

The Post Service is an ASP.Net Core application that provides an API for creating and retrieving posts. The Dockerfile for the Post Service looks like this:

```Dockerfile
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER app
WORKDIR /app
EXPOSE 8081
ENV ASPNETCORE_URLS=http://+:8081

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["Post-Service/Post-Service.csproj", "Post-Service/"]
RUN dotnet restore "./Post-Service/Post-Service.csproj"
COPY . .
WORKDIR "/src/Post-Service"
RUN dotnet build "./Post-Service.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./Post-Service.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Post-Service.dll"]
```

This Dockerfile sets up the Post Service to run on port 8081 and copies the built application into the container.

## Docker Compose
Docker Compose is a tool for defining and running multi-container Docker applications. With Compose, you use a YAML file to configure your application's services. Then, with a single command, you create and start all the services from your configuration.

In my case, I have a docker-compose.yml file that defines the services that make up my application so they can be run together in an isolated environment.

Here is the docker-compose.yml file:

```yaml
version: '3'
services:
   zookeeper:
      container_name: zookeeper
      image: 'confluentinc/cp-zookeeper:latest'
      environment:
         ZOOKEEPER_CLIENT_PORT: 2181
         ZOOKEEPER_TICK_TIME: 2000
      ports:
         - '2181:2181'
      networks:
         - backend

   kafka:
      container_name: kafka
      image: 'confluentinc/cp-kafka:latest'
      depends_on:
         - zookeeper
      ports:
         - '9092:9092'
         - '29092:29092'
      environment:
         KAFKA_BROKER_ID: 1
         KAFKA_ZOOKEEPER_CONNECT: 'zookeeper:2181'
         KAFKA_ADVERTISED_LISTENERS: 'PLAINTEXT://kafka:9092,PLAINTEXT_HOST://localhost:29092'
         KAFKA_LISTENER_SECURITY_PROTOCOL_MAP: 'PLAINTEXT:PLAINTEXT,PLAINTEXT_HOST:PLAINTEXT'
         KAFKA_INTER_BROKER_LISTENER_NAME: PLAINTEXT
         KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR: 1
      networks:
         - backend

   frontend:
      container_name: frontend
      build:
         context: ./Frontend
         dockerfile: Dockerfile
      ports:
         - '3000:3000'
      depends_on:
         - api-gateway
      networks:
         - backend

   api-gateway:
      container_name: api-gateway
      build:
         context: ./API-Gateway
         dockerfile: ./API-Gateway/Dockerfile
      ports:
         - '8080:8080'
      depends_on:
         - kafka
      networks:
         - backend

   post-service:
      container_name: post-service
      build:
         context: ./Post-Service
         dockerfile: ./Post-Service/Dockerfile
      ports:
         - '8081:8081'
      depends_on:
         - kafka
      networks:
         - backend

networks:
   backend:
      name: backend
```
This `docker-compose.yml` file defines the services that make up my application, including Zookeeper, Kafka, the frontend, the API Gateway, and the Post Service. It sets up the necessary environment variables and ports for each service and specifies the dependencies between the services.

In this docker-compose.yml file are the following services:

zookeeper
: This service uses the confluentinc/cp-zookeeper:latest image and is named zookeeper. It exposes port 2181 and is part of the backend network. The environment variables ZOOKEEPER_CLIENT_PORT and ZOOKEEPER_TICK_TIME are set to 2181 and 2000 respectively.

kafka
: This service depends on the zookeeper service. It uses the confluentinc/cp-kafka:latest image and is named kafka. It exposes ports 9092 and 29092 and is part of the backend network. Several environment variables are set to configure Kafka.

frontend
: This service depends on the api-gateway service. It builds from a Dockerfile located in the ./Frontend directory and is named frontend. It exposes port 3000 and is part of the backend network.

api-gateway
: This service depends on the kafka service. It builds from a Dockerfile located in the ./API-Gateway directory and is named api-gateway. It exposes port 8080 and is part of the backend network.

post-service
: This service depends on the kafka service. It builds from a Dockerfile located in the ./Post-Service directory and is named post-service. It exposes port 8081 and is part of the backend network.

networks
: A network named backend is defined. All services are part of this network, allowing them to communicate with each other.

This file tells Docker to:
- Build the image for each service using the Dockerfile in the specified location.
- Map the ports so that the services can communicate with one another and with the host machine.
- Run the services in isolated containers.

{type="none"}

Once the `docker-compose.yml` file is set up, you can start your application with the `docker-compose up` command, which will create and start all the services defined in the configuration.

![](docker_desktop_compose.png)