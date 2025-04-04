# Fullstack Docker

This will create a Docker Compose pipeline including React Front End, Express API Backend, NGINX Reverse Proxy Server for React and Express services, MySQL Database and admin interface for MySQL.
## Getting Started

### Prerequisites

- Docker
- Docker Compose

### Installation

   1. Clone the repository:

   ```bash
   git clone https://github.com/kietnguyen335445/fullstack-docker.git
   cd fullstack-docker
   ```
   2. Build and start the Docker containers:
```bash
docker-compose up --build
```
Usage

After starting the Docker containers, you can access the application at http://localhost:3000.

Here are all the services that we can test:

http://localhost:3000 is the React client

http://localhost:5050 is the Express API

http://localhost:8081 is the client proxied from NGINX server

http://localhost:8080 is the Adminer MySQL admin interface

Server: "db"

Username: "MYSQL_USER"

Password: "MYSQL_PASSWORD"
