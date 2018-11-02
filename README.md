
# fuzzy-fortnight
This provides provides a starting point for understanding common Microservice architecture patterns by example of a proof-of-concept application built with Spring Boot, Spring Cloud, and Docker.

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


### System Diagram
![alt text](https://github.com/gramcha/fuzzy-fortnight/blob/master/fuzzy-fortnight.jpg)
