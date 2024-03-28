# Project Overview: DailyGrind

DailyGrind is a community platform designed for coffee enthusiasts to share their knowledge and experiences. It's a place where users can create blog posts, tutorials, guides, and more, all centered around the world of coffee.

## Architecture

The platform is composed of several microservices, each serving a specific function. These services include:

- **Frontend**: A React application that serves the user interface for the platform.
- **API Gateway**: An ASP.Net Core application that acts as a reverse proxy for the other services.
- **Post Service**: An ASP.Net Core application that provides an API for creating and retrieving posts.

These services communicate with each other using Kafka, a distributed streaming platform. The entire application is containerized using Docker, which allows for easy deployment and scaling.

## Tech Stack

- **Frontend**: The frontend is built with React, a popular JavaScript library for building user interfaces. The application is served by an Nginx server running in a Docker container.

- **Backend Services (API Gateway and Post Service)**: The backend services are built with ASP.Net Core, a framework for building web applications. Each service runs in its own Docker container.

- **Data Streaming**: Kafka is used for inter-service communication. It allows the services to publish and subscribe to streams of records in a fault-tolerant way.

- **Containerization**: Docker is used to containerize the application. This makes it easy to build, ship, and run the application across different environments.

- **Orchestration**: The application is orchestrated using Docker Compose locally and Kubernetes for production. Docker Compose allows defining and running multi-container Docker applications, while Kubernetes provides a platform for automating deployment, scaling, and management of containerized applications.

## App structure

The application is divided into three main applications: Frontend, API Gateway, and Post Service. Each backend service has its own responsibilities and communicates with the others through Kafka.

- **Frontend**: The frontend service is responsible for serving the user interface of the application. It communicates with the API Gateway to fetch and display data.
- **API Gateway**: The API Gateway service acts as a reverse proxy for the other services. It receives requests from the frontend and forwards them to the appropriate service.
- **Post Service**: The Post Service provides an API for creating and retrieving posts. It communicates with the API Gateway to handle requests from the frontend.

The services are designed to be decoupled and independently scalable. This allows for easy maintenance and scaling of the application. The application structure is as follows:

## App Diagram

This diagram shows the architecture and flow of the application and which services communicate with each other. It provides a high-level overview of the application structure:

![](app_diagram.png)

## Folder Structure

The application is organized into the following local folders:

```
.
└── Individual-project/
    ├── docker-compose.yml
    ├── Frontend/
    │   ├── src
    │   ├── public
    │   └── Dockerfile
    ├── API-Gateway/
    │   ├── Controllers
    │   ├── Middleware
    │   └── API-Gateway/Dockerfile
    └── PostService/
        ├── Controllers
        ├── Middleware
        └── PostService/Dockerfile
```