Tasks
---
#### 1. the services are configured using the “Configuration as code” approach figure out how, where is the configuration stored and find out how to modify it and try it (1 point)

The configuration is stored in git repository. URL of the repository is defined in `spring-petclinic-config-server/src/main/resources/bootstrap.yml`
 as a spring.cloud.config.server.git.uri property.
 
I forked the repository and changed the URL to `https://github.com/jakubgruber/spring-petclinic-microservices-config`.
I added `custom.message` property to `api-gateway.yml` and used in it `ApiGatewayController.java`.

Logged output from the service when opening any pet owner:

`api-gateway          | 2020-03-14 15:05:57.334  INFO [api-gateway,60115b3a22e6ae74,60115b3a22e6ae74,true] 18 --- [or-http-epoll-1] o.s.s.p.a.b.web.ApiGatewayController     : Custom message - hello from cloud config - Jakub Gruber`

--- 
        
#### 2. the services must be started in proper order as stated in the readme file. However, docker-compose cannot guarantee the adequate start order can you figure out how this can be achieved? (0.5 pt)

The dependency between containers is achieved by `https://github.com/jwilder/dockerize` tool. In `docker-compose.yml`,
we can see that each service is started as follows `["./dockerize","-wait=tcp://discovery-server:8761", "-timeout=60s", ...]`.
The wait flag specifies which service to start and checks if the given tcp port is up or not.

---

#### 3. how can I make one service wait for others to start? (0.5 pt)

Again, via dockerize plugin. Below is an example how to wait for multiple services, even for http, tcp up states or for a file being generated.

`dockerize -wait tcp://db:5432 -wait http://web:80 -wait file:///tmp/generated-file`
   
---   
        
#### 4. the services are using an in-memory database; the data will be lost when the “network” is stopped. Change whatever must be changed to stored data for each service in a separate database (1 point)

---

Extra tasks
---

#### 1. collect the logs (1 point) (and eventually the metrics, 2 points) transparently and store them in ElasticSearch

#### 2. can you optimize the Dockerfile (docker/Dockerfile) so the image will be created only once? (2 points)
