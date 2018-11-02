
# fuzzy-fortnight
This project a starting point for understanding common Microservice architecture patterns by example of a proof-of-concept application built with Spring Boot, Spring Cloud, and Docker.

### Tech Stack
The Technical Stack used for this service is,
- Language - JAVA
- Spring Boot Web Application
- Spring Cloud
- Netflix OSS
- Docker



### Code Structure
This project is structured like below directory tree.
```sh
|-microservices
   |---composite
   |-----word-query-service
   |---core
   |-----antonyms-query-service
   |-----sounds-like-query-service
   |-----synonyms-query-service
   |---support
   |-----auth-service
   |-----eureka-discovery-service
   |-----hystrix-dashboard-service
   |-----turbine-aggregator-service
   |-----zuul-edge-service

```

Each component inside the composite, core and support folders are maven modules and git sub modules.

#### Requirement 1

As an end user, I want to know the synonyms of a given word. It requires authentication and user role is normal.

Example -
```
Input - happy

Output - content
```

#### Requirement 2

As an end user, I want to know the antonyms of a given word. It requires authentication and user role is admin.

Example -
```
Input - late

Output - early
```

#### Requirement 3

As an end user, I want to know the sounds like of a given word. It requires authentication and user role is normal.

Example -
```
Input - elefint

Output - elephant
```

### System Diagram
![alt text](https://github.com/gramcha/fuzzy-fortnight/blob/master/fuzzy-fortnight.jpg)

### Infrastructure Services
eureka-discovery-service - It discovers the rest of the services in this system. All the other systems register them into this service. This is basically server side discovery pattern. We are using the Netflix Eureka server for that. This is developed using Spring Boot, Cloud with Eureka server.

**zuul-edge-service** - It is an edge service which is exposed to the public internet. It acts as a gateway to all other micro services. We are using the Netflix Zuul for that. It has routes for functional services. It also makes authorisation based on the bear token in request. It is developed using Spring Boot, Cloud, Security with Zuul.

**ribbon** - It is part of zuul edge service where zuul uses for routing the request to more than one instance.

**circuit-breaker** - It is enabled in composite service and other functional services for fallback mechanism incase of exception.

**turbine-aggregator-service** - It is an aggregator service which will collect the hystrix details from all the composite and functional services and maintain that is stream of events for 2 mins(by default).

**hystrix-dashboard-service** - It is an user interface service and provide nice representation on turbine stream of events.

### Functional Services
We are going to decompose our business requirement into services. There are a number of strategies that can help:
```
- Decompose by business capability and define services corresponding to business capabilities.
- Decompose by domain-driven design subdomain.
- Decompose by verb or use case and define services that are responsible for particular actions. e.g. an authenticating service that’s responsible for authenticating users.
- Decompose by nouns or resources by defining a service that is responsible for all operations on entities/resources of a given type. e.g. a synonyms service that is responsible for giving the synonym of given word.
```
In our case, we will have the following services

**auth-service** - It contains the authentication logic using spring security. It creates the Json based Web Tokens(JWT) for the successful authentication. That has to be used in sub sequent other service call for authorisation. It has two types of roles namely normal user and admin user.

**antonyms-query-service** - It provides the antonym result to the given word. Ideally it should have some DB for keeping the list words and its antonyms results. But I don't want to build any data store for that. So I am going to use the Datamuse for getting antonyms result. This application is developed using Vertx web and it is an asynchronous web service.Zuul service will route the request only for admin user.

**sounds-like-query-service** - It provides the sounds like result to the given word. Here also I am going to use the Datamuse for getting sounds like result. This application is developed using Spring Boot Web Flux and it is an asynchronous web service.

**synonyms-query-service** - It provides the synonyms result to the given word. Here also I am going to use the Datamuse for getting synonyms result. This application is developed using Spring Boot Web and it is a synchronous web service.



### Data Consistency

We need to maintain the data consistency across services. Maintaining data consistency between services is a challenge because 2 phase-commit/distributed transactions is not an option for many applications. In order to ensure loose coupling, each service has its own database. But for simplicity instead of DB I am going to use the Datamuse as my source of truth.
