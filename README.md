# Fullstack Docker

This project sets up a full-stack application using Docker Compose, integrating a React frontend, Express API backend, NGINX reverse proxy server, MySQL database, and an admin interface for MySQL.

## Features

- **React Frontend:** Serves the user interface.
- **Express API Backend:** Handles business logic and API requests.
- **NGINX Reverse Proxy:** Routes traffic between the frontend and backend services.
- **MySQL Database:** Stores application data.
- **Admin Interface for MySQL:** Provides a web-based interface to manage the MySQL database.

## Getting Started

### Prerequisites

Ensure you have the following installed on your system:

- Docker
- Docker Compose

### Project Structure

The repository is organized as follows:

- `api-server/`: Contains the Express API backend code.
- `blog-ui/`: Contains the React frontend code.
- `nginx/`: Contains the NGINX configuration.
- `.gitignore`: Specifies files and directories to be ignored by Git.
- `README.md`: This documentation file.
- `docker-compose.yml`: Defines the services, networks, and volumes for Docker Compose.

### Services Overview

The `docker-compose.yml` file defines the following services:

- **db:** A MySQL database service.
- **nginx:** An NGINX reverse proxy service.
- **api:** The Express API backend service.
- **ui:** The React frontend service.

### Environment Variables

The project uses environment variables for configuration. Create a `.env` file in the root directory and define the following variables:

```bash
MYSQL_DATABASE=your_database_name
MYSQL_USER=your_mysql_user
MYSQL_PASSWORD=your_mysql_password
MYSQL_ROOT_PASSWORD=your_mysql_root_password
MYSQL_HOST=your_mysql_host
API_PORT=your_api_port
CLIENT_PORT=your_client_port
```

Docker Compose Configuration

```bash
version: '3.8'
x-common-variables: &common-variables
  MYSQL_DATABASE: $MYSQL_DATABASE
  MYSQL_USER: $MYSQL_USER
  MYSQL_PASSWORD: $MYSQL_PASSWORD

services:
  db:
    image: mysql
    restart: always
    cap_add:
      - SYS_NICE
    volumes:
      - mysql_data:/var/lib/mysql
      - ./api-server/db-setup.sql:/docker-entrypoint-initdb.d/setup.sql
    ports:
      - "9906:3306"
    environment:
      <<: *common-variables
      MYSQL_ROOT_PASSWORD: $MYSQL_ROOT_PASSWORD
      MYSQL_ROOT_HOST: $MYSQL_HOST 

  nginx:
    depends_on:
      - api
      - ui
    restart: always
    build: 
      context: ./nginx
    ports:
      - "8081:80"

  api:
    build: 
      context: ./api-server
      target: dev
    depends_on:
      - db
    volumes:
      - ./api-server:/src 
      - /src/node_modules
    command: npm run start:dev
    ports:
      - $API_PORT:$API_PORT
    environment:
      <<: *common-variables
      PORT: $API_PORT
      NODE_ENV: development

  ui:
    stdin_open: true
    environment:
      - CHOKIDAR_USEPOLLING=true
    build:
      context: ./blog-ui
    volumes:
      - ./blog-ui:/src
      - /src/node_modules
    ports:
      - $CLIENT_PORT:$CLIENT_PORT
  
  adminer:
    image: adminer:latest
    restart: unless-stopped
    ports:
      - 8080:8080
    depends_on:
      - db
    environment:
      ADMINER_DEFAULT_SERVER: db

volumes:
  mysql_data:
  ```

Installation
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

http://localhost:3000 is the React client.

http://localhost:5050 is the Express API.

http://localhost:8081 is the client proxied from NGINX server.

http://localhost:8080 is the Adminer MySQL admin interface.

Adminer Configuration

Server: "db"

Username: "MYSQL_USER"

Password: "MYSQL_PASSWORD"

3. Stopping the Application

To stop the running services, execute:

```bash
docker-compose down
```
