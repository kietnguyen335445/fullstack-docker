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

Project Structure
/frontend: Contains the frontend React application.

/backend: Contains the backend Node.js application.

/database: Contains database configurations and migration scripts.

docker-compose.yml: Docker Compose configuration file.

Dockerfile: Dockerfile for building the application container.
