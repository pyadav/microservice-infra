# Project Overview
We are using docker container to manage all our services - `Users`, `Products`, and `Orders` with **Nginx** acting as a reverse proxy to expose these services via port `80`. All service communicate with **PostgreSQL** database, and all of them perform basic CRUD (Create, Read, Update, Delete) operations. 


## Table of Contents
- [Project Overview](#project-overview)
  - [Table of Contents](#table-of-contents)
  - [Getting Started](#getting-started)
    - [Prerequisites](#prerequisites)
    - [Clone the repository](#clone-the-repository)
  - [Project Architecture](#project-architecture)
  - [Docker Setup](#docker-setup)
    - [Running the Project](#running-the-project)
  - [Nginx Configuration](#nginx-configuration)
  - [Service Endpoints](#service-endpoints)
    - [Users Service](#users-service)
    - [Products Service](#products-service)
    - [Orders Service](#orders-service)

## Getting Started

To get started with this project, you'll need to have Docker and Docker Compose installed on your machine.

### Prerequisites

- Docker [Install Docker](https://docs.docker.com/get-docker/)
- Docker Compose [Install Docker Compose](https://docs.docker.com/compose/install/)

### Clone the repository

```bash
git clone https://github.com/pyadav/microservice-infra
cd microservice-infra
```

## Project Architecture
- **Nginx**: Serves as a reverse proxy or gateway for the `Users`, `Products`, and `Orders` services.
- **PostgreSQL**: Database used by all services to persist data.
- **Docker Compose**: Manages the containerized services.

Each service runs independently and exposes REST API endpoints for CRUD operations. These services are available through the Nginx reverse proxy.

## Docker Setup

Docker Compose setup is defined in the `docker-compose.yml` file. It spins up the following containers:

- **gateway**: Nginx container that routes requests to the appropriate services.
- **postgres**: Database to store information for all services.
- **users**
- **orders**
- **products**

### Running the Project

1. Ensure you have Docker and Docker Compose installed.
2. Build and start the containers:

```bash
docker-compose up --build
```

3. The Nginx reverse proxy will expose the services on port `80`.

You can now access the services through the following endpoints:
- Users: `http://localhost/users`
- Products: `http://localhost/products`
- Orders: `http://localhost/orders`

## Nginx Configuration

Nginx is configured to proxy traffic to the services running on different internal ports:

```nginx
events {}

http {
    server {
        listen 80;

        location /users {
            proxy_pass http://users-service:3000/users;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;

            # Timeout settings
            proxy_connect_timeout 60s;
            proxy_send_timeout 60s;
            proxy_read_timeout 60s;
            send_timeout 60s;

            # Keep connections alive
            keepalive_timeout 65s;
        }

        location /orders {
            proxy_pass http://orders-service:3100/orders;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;

            # Timeout and keepalive settings
            proxy_connect_timeout 60s;
            proxy_send_timeout 60s;
            proxy_read_timeout 60s;
            send_timeout 60s;
            keepalive_timeout 65s;
        }

        location /products {
            proxy_pass http://products-service:3200/products;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;

            # Timeout and keepalive settings
            proxy_connect_timeout 60s;
            proxy_send_timeout 60s;
            proxy_read_timeout 60s;
            send_timeout 60s;
            keepalive_timeout 65s;
        }
    }
}
```

## Service Endpoints

Each service (`Users`, `Products`, and `Orders`) supports CRUD operations, which can be accessed via their respective endpoints.

### Users Service
- Base URL: `http://localhost/users`
- Exposes user-related API endpoints.
  
- **Create a User**
```bash
curl -X POST http://localhost/users \
-H "Content-Type: application/json" \
-d '{"firstName": "John", "lastName": "Doe"}'
```

- **Get All Users**
```bash
curl http://localhost/users
```

- **Get a User by ID**
```bash
curl http://localhost/users/{id}
```

- **Delete a User**
```bash
curl -X DELETE http://localhost/users/{id}
```

### Products Service
- Base URL: `http://localhost/products`
- Manages product-related operations.

- **Create a Product**
```bash
curl -X POST http://localhost/products \
-H "Content-Type: application/json" \
-d '{"productName": "Hoppscotch", "quantity": 100}'
```

- **Get All Products**
```bash
curl http://localhost/products
```

- **Get a Product by ID**
```bash
curl http://localhost/products/{id}
```

- **Delete a Product**
```bash
curl -X DELETE http://localhost/products/{id}
```

### Orders Service
- Base URL: `http://localhost/orders`
- Handles order-related actions.
  
- **Create an Order**
```bash
curl -X POST http://localhost/orders \
-H "Content-Type: application/json" \
-d '{"isCancelled": false}'
```

- **Get All Orders**
```bash
curl http://localhost/orders
```

- **Get an Order by ID**
```bash
curl http://localhost/orders/{id}
```

- **Delete an Order**
```bash
curl -X DELETE http://localhost/orders/{id}
```