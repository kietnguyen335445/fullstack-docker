# Fullstack Docker

This will create a Docker Compose pipeline including React Front End, Express API Backend, NGINX Reverse Proxy Server for React and Express services, MySQL Database and admin interface for MySQL.
## Getting Started

### Prerequisites

- Docker
- Docker Compose

Docker Compose Config
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
