# Frankenphp
docker run -d --name frankenserver1 -e "SERVER_NAME=:8080" -v ./app:/app/public -p 8080:80 dunglas/frankenphp
