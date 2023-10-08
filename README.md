https://nginxproxymanager.com/setup/#running-the-app


Using MySQL / MariaDB Database
If you opt for the MySQL configuration you will have to provide the database server yourself. You can also use MariaDB. Here are the minimum supported versions:

MySQL v5.7.8+
MariaDB v10.2.7+
It's easy to use another docker container for your database also and link it as part of the docker stack, so that's what the following examples are going to use.

Here is an example of what your docker-compose.yml will look like when using a MariaDB container:
=====================================================================================================
version: '3.8'
services:
  app:
    image: 'jc21/nginx-proxy-manager:latest'
    restart: unless-stopped
    ports:
      # These ports are in format <host-port>:<container-port>
      - '80:80' # Public HTTP Port
      - '443:443' # Public HTTPS Port
      - '81:81' # Admin Web Port
      # Add any other Stream port you want to expose
      # - '21:21' # FTP
    environment:
      # Mysql/Maria connection parameters:
      DB_MYSQL_HOST: "db"
      DB_MYSQL_PORT: 3306
      DB_MYSQL_USER: "npm"
      DB_MYSQL_PASSWORD: "npm"
      DB_MYSQL_NAME: "npm"
      # Uncomment this if IPv6 is not enabled on your host
      # DISABLE_IPV6: 'true'
    volumes:
      - ./data:/data
      - ./letsencrypt:/etc/letsencrypt
    depends_on:
      - db

  db:
    image: 'jc21/mariadb-aria:latest'
    restart: unless-stopped
    environment:
      MYSQL_ROOT_PASSWORD: 'npm'
      MYSQL_DATABASE: 'npm'
      MYSQL_USER: 'npm'
      MYSQL_PASSWORD: 'npm'
    volumes:
      - ./mysql:/var/lib/mysql
=====================================================================================================
Default Administrator User
Email:    admin@example.com
Password: changeme
Immediately after logging in with this default user you will be asked to modify your details and change your password.