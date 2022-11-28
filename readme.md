# TP: Devops
## Prerequisites
- [Docker Desktop](https://www.docker.com/products/docker-desktop/)
- [Git](https://git-scm.com/downloads)
## E01: Creating the development environment
1. Clone spring-petclinic
```shell
git clone https://github.com/spring-projects/spring-petclinic.git
```
2. Prepare the development environment with VS code and the dev-container extension
## E02: Application launch
### DEV
1. Launch the application with a base in memory
2. Launch the application with the spring mysql profile `spring.profiles.active=mysql`, what did you notice?
3. Create a container with a mysql database
   - Do a search on [Docker Hub](https://hub.docker.com/search?q=mysql) to find the right image
```shell
docker run -d --name mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -p 3306:3306 ?
```
4. Relaunch the application with the mysql profile.

## E04: Simulation of a second environment
2. Launch another instance of the database listening on port `3307`
3. Launch another instance of the application on port `8081`

## E05: Containerize the application

1. Add a Dockerfile and create an image containing the java application (see this [example](https://www.docker.com/blog/9-tips-for-containerizing-your-spring-boot-code/) for more details).

```dockerfile
FROM eclipse-temurin:17-jre-jammy
ARG JAR_FILE=target/*.jar
COPY ${JAR_FILE} app.jar
EXPOSE 8080
ENTRYPOINT["java", "-jar", "/app.jar"]
```
2. Try to optimize the creation of the image.
3. Launch a container from the created image (use the `--rm` option)
4. Launch a container passing the argument `--spring.profiles.active=mysql`
4. Launch a container passing the environment variable `spring.profiles.active=mysql`
6. Create a container for the application and a container for the database and attach them to a bridge type network.


## E06: Using Docker compose
1. Use docker compose to create two environments dev and qualif

At the level of each environment we must have two containers, a container for the database and a container for the application.


2. Create a production environment with two instances of the application and one instance of the database.

3. [Homework] Secure access to the prod environment by adding a reverse-proxy [nginx](https://www.nginx.com), the only port that should be exposed to the outside is the port 443(https) of the reverse-proxy, connections to port 80 must be redirected to port 443.

## E07: Check application logs
Check logs at docker container level


## E08: Check gracefull shutdown
1. Launch the java application locally then try to stop it with the `kill` command
2. Add a ShutdownHook at the java application level and relaunch it
```java
Runtime.getRuntime().addShutdownHook(new Thread(() -> {
System.out.println("########## ShutdownHook Started ... ###########");
}));
```
3. Try to stop it, again with the `kill` command
4. Add a `sleep` instruction of 20s and repeat the test.
4. Redo the test with the command `kill -9`
5. Launch the application in a container then stop the container with the command `docker stop`
6. Redo the test by changing the waiting time with the `--time` option