# Docker / PHP 8 + MySQL

## :penguin: 1.0 Install Docker on Ubuntu

### 1.1 Update packages and install dependencies

```sh
$ sudo apt update && sudo apt upgrade -y
$ sudo apt install -y apt-transport-https ca-certificates curl software-properties-common
```

### 1.2 Add the official Docker repository

```sh
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
$ echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

### 1.3 Install Docker

```sh
$ sudo apt update
$ sudo apt install -y docker-ce docker-ce-cli containerd.io
```

### 1.4 Verify the installation / Enable Docker

```sh
$ sudo systemctl enable docker
$ sudo systemctl start docker
$ sudo docker --version
```

### 1.5 Add user to the Docker group (Optional, avoids using sudo)

```sh
$ sudo usermod -aG docker $USER
$ newgrp docker
```

## :whale2: 2.0 Install Docker Compose

```sh
$ sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
$ sudo chmod +x /usr/local/bin/docker-compose
$ docker-compose --version
```

## :whale2: 3.0 Create the Docker environment with PHP 8 and MySQL

### 3.1 Create a directory for the project and navigate to it

```sh
$ mkdir docker-php
$ cd docker-php
```

### 3.2 Create a docker-compose.yml file

```sh
$ nano docker-compose.yml
```

### 3.3 Add the following content

```sh
version: "3.8"

services:
  php:
    image: php:8.0-apache
    container_name: php_container
    restart: always
    ports:
      - "8080:80"
    volumes:
      - ./www:/var/www/html
    depends_on:
      - db

  db:
    image: mysql:8
    container_name: mysql_container
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: database
      MYSQL_USER: user
      MYSQL_PASSWORD: password
    ports:
      - "3306:3306"
    volumes:
      - mysql_data:/var/lib/mysql

volumes:
  mysql_data:
```

## :card_file_box: 4.0 Create the project directory

### 4.1 Create a folder to store PHP files

```sh
$ mkdir www
$ echo "<?php phpinfo(); ?>" > www/index.php
```

## :whale2: 5.0 Run Docker

### 5.1 Start the containers

```sh
$ docker-compose up -d
```

### 5.2 Check the running containers

```sh
$ docker ps
```

### 5.3 Access PHP in the browser

```sh
$ http://localhost:8080
```

### 5.4 Connect to MySQL via terminal

```sh
$ docker exec -it mysql_container mysql -u user -p
```

## :x: 6.0 Stop and Remove Containers

### 6.1 To stop the containers

```sh
$ docker-compose down
```

### 6.2 To remove everything (including volumes)

```sh
$ docker-compose down -v
```
