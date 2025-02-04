# Frankenphp
## FrankenPHP mode runtine biasa
#### project-folder/
#### ├── docker-compose.yml
#### ├── app/
####     └── index.php
    
docker run -d --name frankenserver1 -e "SERVER_NAME=:8080" -v ./app:/app/public -p 8080:80 dunglas/frankenphp
<code>
version: '1.0'

services:
  frankenphp:
    image: dunglas/frankenphp:latest
    container_name: Project01
    volumes:
      - ./app:/app/public
    ports:
      - "8080:80"
    environment:
      - "SERVER_NAME=:8080"
</code>
