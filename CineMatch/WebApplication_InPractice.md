# A FULL STACK WEB APPLICATION: CineMatch 
Working with a full stack application and applying UX (IPS)


## Table of Contents
- [1. Introduction](#1-introduction)
- [2. Frontend](#2-frontend)
- [3. Distributed Communication](#3-distributed-communication)
- [4. Backend](#4-backend)
	- [4.1 Microservices with Spring Boot](#41-microservices-with-spring-boot)
		- [4.1.1 Future plans](#411-future-plans)
	- [4.2 Gateway and Keycloak](#42-gateway-and-keycloak)
	- [4.3 API Documentation](#43-api-documentation)
- [5. Data persistence](#5-data-persistence)
	- [5.1 SQL and NoSQL databases](#51-sql-and-nosql-databases)
	- [5.2 Testing with different databases](#52-testing-with-different-databases)
	- [5.3 Hibernate (ORM)](#53-hibernate-orm)
	- [5.4 Movies and cinema data](#54-movies-and-cinema-data)
- [6. User Experience](#6-user-experience)
	- [6.1 Heuristics of choice](#61-heuristics-of-choice)
	- [6.2 High fidelity mockups / Mock-up (High fidelity)](#62-high-fidelity-mockups--mock-up-high-fidelity)
	- [6.3 Usability testing](#63-usability-testing)
		- [6.3.1 Participants](#631-participants)

## 1. Introduction

This document is structured to feature everything I have done for **Learning Outcome 01: Web
Application** during the IPS course. I spend most of the semester on the backend and data persistence, as it required a lot of work and I was also very new to the technologies. 


I used footnotes to add any additional information on sources I have used. 

Bold and underlined text in the paragraphs is clickable and will take you to the referenced section.
## 2. Frontend

<!-- TODO: Write about everything that is mentioned below and provide examples from the project or tutorials I followed -->


Using: React - JavaScript
Talk about: React tutorials, no time to learn TypeScript & too big of a learning curve, React's popularity & lightweight library, Materialize, perhaps the OMDb tutorial


## 3. Distributed Communication

<!-- TODO: Write this section, talk about what is mentioned below -->


Concurrency research and extra information about when to use HTTPS and JSON.
When I started using Docker, things initially became more complex but I ended up getting it to work.

Talking about why I used REST...
<!--
Some use cases for both HTTPS and JSON:

HTTPS:

-   Any situation where sensitive data is being transmitted over the internet, such as login credentials, payment information, or personal data.
-   Any situation where the data being transmitted needs to be kept confidential and secure, such as in a healthcare application.
-   Any situation where the integrity and authenticity of the data being transmitted needs to be ensured, such as in an online banking application.

JSON:

-   Any situation where structured data needs to be exchanged between systems or applications, such as in a REST API.
-   Any situation where the data being transmitted needs to be lightweight and easily parsable, such as in a mobile app or web application.
-   Any situation where the data being transmitted needs to be easily readable and editable by developers, such as in a configuration file.
-->
## 4. Backend
This semester, I took on the challenge of building a complex backend using Spring Boot with Java. It was an opportunity for me to dive into new concepts and technologies, especially when I decided to build a microservices architecture. The books I am currently reading, The Pragmatic Programmer and Clean Code, both feature Java-based examples and since I have worked a lot with C# in the past, it seemed to be the right decision to use Java for the backend. I probably spend most of the semester on building the backend, but it paid off as it eventually came out successful and is fully functional.


### 4.1 Microservices with Spring Boot
Spring Boot has a low learning curve and provides a wide range of features that simplify development, making it easier for me to get started, especially since I was new to Java. There are also a lot of great tutorials out there to help you get started. For my project, the tutorial on microservices by Java Brains was especially important[^1].

The backend architecture of CineMatch consists of five microservices, each serving a specific purpose within the application. The core microservices include _user-data_, _user-preferences_, and _user-matching_. These services handle user-related data and the matching algorithm. In addition to these core microservices, the architecture also required two additional services: the *api-gateway* and a *discovery server*. These services are needed for the routing and 'finding' of the microservices. 

In the last semester, the group project consisted of just three services: frontend, backend and an additional service for scraping information. I learned the concept of distributed web applications, but other concepts associated with distributed web applications, like scalability were still a bit vague to me. This is why I decided to use a microservices architecture.

The microservices architecture provided me with new insights in why a stable architecture is important, it revealed details to me that I initially missed with other projects. By making smaller services, the idea of scalability, maintainability, and separation of concerns becomes much more clear and I learned a lot from that.

Overall, building the backend with Spring Boot and using a microservices architecture was a big learning opportunity for me and allowed me to explore new concepts and technologies while working with Java. 

#### 4.1.1 Future plans

If I had more time, I would have added two additional services: One service would be Movie-Data-Service and the other one Cinema-Data-Service. I planned to develop a Python-based web scraper, which would retrieve data from cinema websites based on user-entered cinema names. A small attempt I did at making this a reality is by successfully configuring a demo to use OMDb to retrieve movie data. 

### 4.2 Gateway and Keycloak

Although the API gateway technically is not required, it is better for security and load balancing. The gateway can take care of security by handling authentication and authorization, protecting the microservices from unauthorized access. It is able to do its job in load balancing by distributing incoming requests across multiple instances of the microservices to optimize performance and handle increased traffic effectively. 

The API gateway in my application delegates the authentication and authorization to Keycloak, which is properly configured. However, as mentioned in the [**Backend part of the CI/CD section of the Reader's Guide**](https://github.com/mdaveijk/S3-Portfolio/tree/main#232-Backend), this caused some issues in the CI-pipeline and required Testcontainers in order to work, which I did not have time for to implement. As a result, Keycloak is disabled for the final submission of this project to properly showcase the application. 
You can read more about my experience with Keycloak in my [**Security Research**](Research/Security_Research_Identification_and_Authentication_Failures.md) starting under the section [**Possible solution: Using a Single Sign-On Service**](Possible solution: Using a Single Sign-On Service](Research/Security_Research_Identification_and_Authentication_Failures.md#5-possible-solution-using-a-single-sign-on-service).

### 4.3 API Documentation
In the [**README file of the backend monorepository**](https://github.com/mdaveijk/S3-IP-CineMatch/blob/main/README.md), I provided some instructions for setting up the services with Docker. Once the services are up and running, users can access the API documentation and interact with the API. I implemented the API documentation using Swagger/OpenAPI, which provides a clear and user-friendly interface for other developers to use. In addition, users can easily switch between my microservices and view their API documentation. 

![Screenshot of the user-matching-service API documentation. Provided by Swagger.](Media/match_controller_documentation.png)
_Screenshot of the user-matching-service API documentation. Provided by Swagger._

Each controller request of every service is annotated with helpful HTTP status information, which is automatically picked up by Swagger and displayed to users in the API documentation. This helps users understand the expected status codes for each request.

For example, the following image shows an example of a Match DELETE request in Swagger:

![Example of what a DELETE request looks like in Swagger. Part of the user-matching-service.](Media/match_delete_documentation.png)
_Example of what a DELETE request looks like in Swagger. Part of the user-matching-service._


## 5. Data persistence
I did a lot of work for data persistence during this semester, as I made a nice start with it during the last semester, I wanted to expand on that knowledge and learn more about it. I did this by trying out different types of databases for the different microservices. The following sections describe everything I did in detail on the subject of data persistence.

### 5.1 SQL and NoSQL databases
In the previous semester, I did a lot of research on the differences and use cases for SQL and NoSQL databases, which helped me a lot in this semester. As a personal challenge, I decided to find a use-case to use both types of databases into my architecture: MongoDB as a NoSQL database and MariaDB as a SQL database.

To better illustrate this, I made an Entity Relationship Diagram:

![Entity Relationship Diagram - CineMatch](Media/Final_ERD.png)
*Current version of the ERD.*

Colours represent different locations/databases (a legend is added for clarity). For example, the microservices for *Preferences* and *Matching* have a NoSQL database because they're likely often going to change and in case more space is needed, NoSQL databases are also easier to scale, considering they scale horizontally. The *User* microservice on the other hand, is more likely to have a steady structure and might require complex queries in the long run, so a relational-database seemed more suitable. 

### 5.2 Testing with different databases
When it comes to testing, there are different approaches you can take. While setting up a containerized database like MariaDB with Testcontainers is an option, not only was I unable to further explore this option due to time constraints, but it is also a less common approach to the problem. I found out that most Java applications use a in-memory database, like H2, to conduct the testing on instead.[^2] It was a bit tricky to get this to work, but eventually after adding "Spring profiles" it did the trick.

![Configuration class responsible for setting up the test database in the user-data-service.](Media/configure_test_database.png)
_Configuration class responsible for setting up the test database in the user-data-service._

### 5.3 Hibernate (ORM)
The good news is that even though I am using different types of databases, I can use the same ORM (Object-Relational Mapping) framework for both. Hibernate, being a popular choice for Java applications, provides convenient features for performing CRUD operations and entity mapping.

The underlying framework that uses or "consumes" Hibernate differs depending on the database type. For SQL databases, the commonly used framework is JPA (Java Persistence API). In the case of NoSQL databases, specifically MongoDB, it uses the Spring Data MongoDB framework. For example, the _user-preferences-service_ uses the following code to enable the MongoDB framework:
```
@EnableMongoRepositories
public interface PreferencesRepository extends MongoRepository<UserPreferences, Long> {
    
}
```
``
While the _user-data service_ uses:
```
@RepositoryRestResource
public interface UserRepository extends JpaRepository<User, Long> {

}
```

Both JPA and Spring Data MongoDB each have their own syntax and specific annotations, but they essentially result in the same outcome. We can see Hibernate as a form interface that we interact with by using these annotations; we do not have to worry about the underlying technology. Below is a side by side comparison to showcase the differences between the annotations. You can view the full image by clicking on it.

```
SQL annotations example             |  NoSQL annotations example
:-------------------------:|:-------------------------:
![SQL annotations example](Media/hibernate_annotations_sql.png)  |  ![NoSQL annotations example](Media/hibernate_annotations_nosql.png)
```
_Different annotations based on the type of database, side by side._

Finally, the interaction with the repositories remains the same, regardless of the database type:

````
```java
    private PreferencesRepository repository;

    public PreferenceService(PreferencesRepository repository) {
        this.repository = repository;
    }

    public Collection<UserPreferences> getAllPreferences() {
        return this.repository.findAll();
    }
```
````
_Code snippet of the PreferencesService for context._

This code snippet is similar for all my services as they are all working with Hibernate; for example, using methods like ``findAll()`` to return a collection.

This makes the process of interacting with the databases so much easier, yet keeps the way data is handled consistent for different database types. By using Hibernate, my application can easily adapt to different database types in the future without major code modifications or changes to the overall architecture.

### 5.4 Movies and cinema data
As mentioned in the **[Backend](#4-backend)** section, my plan was to expand the application by adding additional services to handle movie and cinema-related data. Using a web scraper, the scraped data would be stored in their respective databases. For movies, I would save only a few essential elements that can serve as identifiers for potentially making another request. This approach aimed to achieve a couple of things: 
1. Avoid exhausting the limited requests available for the OMDb API (because I'm using the free version). 
2. Keep the (redundant) calls to a minimum; only having to query for data once if the movie data already exists.
3. Keeping performance: It is much faster to query a local database than to query an external API every time.  
When a user requires more detailed information about a movie, the system can look up if the movie exists and make an API request to retrieve the additional information from the external source and present it to the user. In short: the goal would be to maintain a lightweight database that contains only the necessary information while keeping performance and no exhausting the resource(s).

## 6. User Experience



---
[^1]: https://www.youtube.com/playlist?list=PLqq-6Pq4lTTZSKAFG6aCDVDP86Qx4lNas
[^2]: https://www.baeldung.com/spring-jpa-test-in-memory-database
