# Frankenphp
## FrankenPHP mode runtine biasa
#### project-folder-docker1/
#### ├── docker-compose.yml
#### ├── app/
####     └── index.php
    
docker run -d --name frankenserver1 -e "SERVER_NAME=:8080" -v ./app:/app/public -p 8080:80 dunglas/frankenphp

## docker-compose
## Membuat file docker-compose.yml
berikut ini file dari docker-compose.yml 
```
version: '1.0'

services:
  franmyphp:
    image: dunglas/frankenphp:latest
    container_name: Project01
    volumes:
      - ./app:/app/public
    ports:
      - "8080:80"
    environment:
      - "SERVER_NAME=:8080"
```
## Membuild docker
```
docker compose build
```
Mengaktifkan docker compose
```
docker compose up -d
```
Menghapus docker
```
docker compose down
```

# Mysql PhpMyadmin dan Frankenphp
## Mendefinisikan Dockerfile
1. Mengambil image docker frankenphp
2. Menambahkan extensi php
```
FROM dunglas/frankenphp:latest

RUN docker-php-ext-install pdo pdo_mysql mysqli
```
## Membuat file docker-compose.yml
```
version: '1.0'

services:
  franmyphp:
    build: .
    container_name: Project01
    volumes:
      - ./app:/app/public
    ports:
      - "8080:80"
    environment:
      - "SERVER_NAME=:8080"
    depends_on:
      mysql:
        condition: service_healthy
        
  mysql:
    image: mysql:5.7
    container_name: mysql
    environment:
      MYSQL_ROOT_PASSWORD: mysqlrootpwd
      MYSQL_DATABASE: database
      MYSQL_USER: username
      MYSQL_PASSWORD: userpassword
      SERVER_NAME: 3306 
    ports:
      - "3306:3306"
    volumes:
      - /public/mysqldata:/var/lib/mysql
    healthcheck:
      test: ["CMD", "mysqladmin", "ping", "-h", "localhost"]
      interval: 10s
      timeout: 5s
      retries: 3
      
  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    container_name: phpmyadmin
    environment:
      PMA_HOST: mysql
      PMA_USER: username
      PMA_PASSWORD: userpassword
      SERVER_NAME: 8585 
    ports:
      - "8585:80"
    depends_on:
      mysql:
        condition: service_healthy

volumes:
  mysql_data:
```
## Upload ke Docker Hub
1. image yang akan di-push harus menyertakan username registry. Misalnya username saya adalah arthawebid, berarti nama images menjadi arthawebid/repository:tag
```sh
docker tag web-franmyphp arthawebid/franmyphp:1.0
```sh
2. Push image ke docker hub
```sh
docker push arthawebid/franmyphp:1.0
```sh
3. setelah terkirim/terupload ke docker hub silahkan edit deskripsi imagenya
