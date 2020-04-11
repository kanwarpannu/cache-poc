# Caching-POC (Using Redis in Docker)

## Introduction 
The application serves as an API to store employee data in a database and make same available in cache.  

1. When **POST** endpoint is hit with employee data it is stored in DB and the cache is updated.  
2. When **GET** endpoint is hit with correct employee id then data is retrieved from cache.  
3. When **GET** endpoint is hit with incorrect employee id then data is checked in both cache and db before returning employee not found message.  
4. When **GET** endpoint is hit then all the employee data is returned only from database.  
5. When **GET** Read-only endpoint is hit with correct employee id then get the employee from cache, if not exist then go to DB but do not persist the data in Cache.  
6. When **GET** Read-only endpoint is hit with incorrect employee id then data is checked in both cache and db before returning employee not found message.  
7. When **PUT(UPDATE)** endpoint is hit with correct employee id then employee data is updated in both cache and Database.  
8. When **PUT(UPDATE)** endpoint is hit with incorrect employee id then data is checked in both cache and db before returning employee not found message.  
9. When **DELETE** endpoint is hit with correct employee id then data is deleted from both cache and database.  
10. When **DELETE** endpoint is hit with incorrect employee id then data is checked in both cache and db before returning employee not found message.  
11. When **DELETE** endpoint is hit then all the employee data is removed from database and cache.

 *In real world scenario we would cache only the most used data (so only **GET** endpoint should store data in cache) otherwise the cache can grow quickly and with non required data.*  

## Requirements
  * Uses **Lombok** - Be sure to enable annotation processing  
  * Uses **Redis** on Docker - Download redis:alpine image from dockerhub  
  * Uses **Spring Cache 'in-memory'** - For Testing purposes only(comes with spring boot starter cache dependency)  
  * Uses **Spring Boot** - version v2.2.4.RELEASE  
  * Uses **Swagger UI** - version v2.9.2
  * Uses **Java** - Version Adopt JDK 1.8.0_232  

## Start Redis
1. Start Redis:
`docker run -d -p 6379:6379 --name my-redis redis`

2. Access redis-cli inside Redis container:
`docker exec -it my-redis redis-cli`  

## Build
  * Build project using `mvn clean install`  

## Access Application  
  * Access application from swagger using base url `http://localhost:9101/swagger-ui.html`  

## Basic Redis commands:  
1. `ping` - To check Redis is up  
Output - "PONG"  
2. `set name mark` - Creates key "name" with value "mark"  
Output - OK  
3. `get name` - Gets value for key "name"  
Output - "mark"  
4. `keys *` - Gets all keys  
5. `del name` - Deletes key "name"  

Use this link to access [Redis-command's-cheatsheet](https://gist.github.com/LeCoupa/1596b8f359ad8812c7271b5322c30946)  

## Dev Notes
Created `@ReadOnlyCacheable` annotation to restrict `@Cacheable` to be read-only. It works like a `@Cacheable` annotation just that it does not insert data into Cache.  