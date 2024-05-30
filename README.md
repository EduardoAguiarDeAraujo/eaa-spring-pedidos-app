# Microservice de Pedidos

## Criando um Container MySQL
```text
docker run -d --name mysqldb -p 3306:3306 -e "MYSQL_ROOT_PASSWORD=root" -v C:\Workspace\docker_volumes\mysql:/var/lib/mysql mysql
```

## Consultando o banco de dados
```text
docker ps
docker exec -it <id-do_container> bash
mysql -u root -p
Enter password:
show databases;
use db-pedidos;
show tables;
describe pedidos;
```

## Configurando o arquivo application.properties
```text
spring.application.name=pedidos-app
spring.datasource.url=jdbc:mysql://root:root@localhost:3306/db-pedidos?createDatabaseIfNotExist=true
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.jpa.database-platform=org.hibernate.dialect.MySQL8Dialect
spring.jpa.hibernate.ddl-auto=update
```

## Criando o arquivo .jar com o Maven
```text
mvn clean
mvn package
```

## Criando o arquivo Dockerfile
```text
FROM openjdk:17-alpine
ARG JAR_FILE=target/*.jar
COPY ${JAR_FILE} app.jar
ENTRYPOINT ["java","-jar","/app.jar"]
```

## Criando a imagem docker e publicando no docker hub
```text
docker build -t eduardoaguiardearaujo/pedidos-app:1.0 .
docker push eduardoaguiardearaujo/pedidos-app:1.0
```

## Criando um container a partir da imagem criada
```text
docker run -d -p 8080:8080 eduardoaguiardearaujo/app-pedidos-ms:1.0
``` 

## Criando o arquivo docker-compose.yml
```yaml
version: '3.9'

services:
  db:
    image: mysql
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: db-pedidos
    ports:
    - "3306:3306"
    volumes:
    - db_data:/var/lib/mysql

  app:
    build: .
    image: eduardoaguiardearaujo/pedidos-app
    environment:
      SPRING_DATASOURCE_URL: jdbc:mysql://db:3306/db-pedidos
      SPRING_DATASOURCE_USERNAME: root
      SPRING_DATASOURCE_PASSWORD: root
    ports:
      - "8080:8080"
    depends_on:
    - db

volumes:
  db_data:
```

## Criando uma imagem e executando comandos com o docker-compose
```text
docker-compose up --build
docker-compose up -d
docker-compose ls
docker-compose ps
docker-compose down 
```
