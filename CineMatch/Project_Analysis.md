# CineMatch Project Analysis
<img src="/Media/cinematchlogo.png" alt="CineMatch logo" width="200">

___
|         |         |
|:---------------:|:---------------:|
|   Release:      |   21th of June 2023      |
|   Version:          |   3.1         |
|   Location:          |   Eindhoven          |
|   Project owner:          |   Maurice Schippers          |
___


## Table of Contents
- [1. Project Description](#1-project-description)
	- [1.1 Motivation](#11-motivation)
- [2. System Architecture Overview](#2-system-architecture-overview)
	- [2.1 The C4-model](#21-the-c4-model)
		- [2.1.1 System Context](#211-system-context-diagram)
		- [2.1.2 Container Diagram](#212-container-diagram)
	- [2.2 Microservices architecture](#22-microservices-architecture)
	- [2.3 Using REST](#23-using-rest)
- [3. User Stories and Quality Measurement](#3-user-stories-and-quality-measurement)
	- [3.1 Definition of Done](#31-definition-of-done)
- [4. User Interface Sketches](#4-user-interface-sketches)
	- [4.1 Homepage](#41-homepage-3-versions)
	- [4.2 Preferences](#42-preferences)
	- [4.3 Matches overview](#43-matches-overview)
	- [4.4 Match browsing](#44-match-browsing)
- [5. Test Methodologies](#5-test-methodologies)

## 1. Project Description
CineMatch is a movie match-making software project that allows users to enter their preferences to watch a certain movie or movie genre, their availability, location and their preferred distance. The system will look for potential matches with other users. Once a match has been made, users can communicate through the application to decide on further details and leave a review about their overall experience afterwards. 
While similar in some respects to popular dating apps, such as Tinder, CineMatch is intended to serve a different purpose.

### 1.1 Motivation
The need for a ‘buddy’ to do activities with is high nowadays. Especially after COVID-19, people are struggling to be more social and finding an activity within a shared interest can be hard. This initiative seeks to alleviate social isolation and encourage social interaction by enabling users to connect with others interested in going to the movies. There are currently no (popular) projects like this, except for a few that have a broad range of activities and not just a cinema visit. This might give users the opportunity to have an app to go to, a central place, when they do not want to be by themselves in the cinema. The project's ultimate goal is to expand beyond the cinema and encompass a broader range of activities.


## 2. System Architecture Overview


### 2.1 The C4-model
The C4-model depicts a visual representation of the architecture of CineMatch. Making it easier to understand and communicate for both technical and non-technical stakeholders. Below are two levels of the model, System Context and the Container diagram. The System Context Diagram gives an overview of all the actors that are relevant for the application. The Container Diagram gives a look inside CineMatch and gives an overview of which elements are part of the entire software system. If required, a Component Diagram could be considered for a future release.

#### 2.1.1 System Context Diagram
<img src="/Media/SystemContext_CineMatch_v1.png" alt="CineMatch Context Diagram" width="600">
The "social cinemagoer" represents the primary stakeholder in the project, or simply put, the end-user. This *actor* interacts with the system and its functionalities. CineMatch makes use of two external APIs: The OMDb API, a RESTful web service to obtain movie information, and a yet to be considered Single Sign-on service. The outcome of my [**Security Research**](/Research/Security_Research_Authentication_Authorization.md) will determine which service this is going to be. Its purpose is to allow users to (safely) sign up for an account on CineMatch with their existing credentials.
The reasoning behind OMDb API as the movie API of choice is because it is very simple to use and is a perfect option considering that it allows 1000 free API requests each month.

#### 2.1.2 Container Diagram
<img src="/Media/ContainerDiagram_CineMatch_v1.png" alt="CineMatch Container Diagram" width="600">
The end-user interacts with the system through the front-end. The front-end communicates through HTTPS/JSON with the API Gateway which in turn then contacts the appropiate microservice with the request from the front-end. Each microservice has its own database based on the data it handles. For example, the Matching microservice has a NoSQL database because I expect the structure to change a lot. The API Gateway is shown to interact with the SSO service, but I have yet to confirm this as of the current release of this analysis and this is purely based on current research and my expectation. 
Finally, the Movie microservice interacts with two seperate APIs, one being the OMDb API to retrieve data about movies and a scraper application specifically for cinema's. The scraper will look for movies current showing in the cinema of which the user requested and return that to the microservice which then requests for data about those movies from the OMDb API. The idea is to store the movie information for a certain time period so it won't have to request the API every time, which would not only be waste of requests but also has an impact on the performance of the application.

### 2.2 Microservices architecture
CineMatch benefits from a microservices architecture for several reasons. Firstly, it allows for better scalability and flexibility. Each microservice can be independently deployed, scaled, and maintained. Efficiently distributing the use of resources and handling increased traffic or workload in specific areas of the system.

Secondly, a microservices architecture promotes loose coupling and modularity. Each microservice is responsible for a specific business goal or functionality, making it easier to understand, develop, and test. Changes or updates to one microservice do not affect the entire system, reducing the risk of unintended consequences and making it easier to introduce new features or enhancements.

Additionally, microservices enable technology diversity. Different microservices can be built using different programming languages, frameworks, or technologies that best suit their specific requirements. This makes it possible to build the scraper application with Python, a language more commonly used to build web scrapers, but can still communicate with the Java applications without issues.

Lastly, microservices contain fault isolation and resilience, especially with the *api-gateway* in place. If one microservice encounters an issue or fails, it does not bring down the entire system. The failure is contained within that specific microservice, and the rest of the system can continue to function. This isolation improves overall system reliability and availability.

### 2.3 Using REST
There is a number of reasons why the components in CineMatch benefit from using REST (Representational State Transfer) as the architectural style for communication. RESTful APIs make it easier for components to communicate and work together by providing a standardized and scalable approach. This promotes easy integration and collaboration between different services.

Using RESTful APIs also enables statelessness, which simplifies the design and scalability of the system. Each request from a client contains all the necessary information for the server to process it, without relying on server-side session state. This allows components to be horizontally scaled and distributed, as requests can be handled independently without the need for shared state. Which is exactly what is needed in order to achieve concurrency within our system.

Lastly, REST promotes loose coupling between components. Each component exposes their own set of resources and endpoints, providing a clear separation of concerns. This decoupling allows components to grow independently, making it easier to introduce changes or add new features without affecting other parts of the system.

At the moment, none of the microservices require WebSockets. REST is sufficient for their communication. The potential use of WebSockets may be considered in the future when implementing the chat functionality within the system.

## 3. User Stories and Quality Measurement

Table 1 shows all (current) CineMatch User Stories sorted by priority. Acceptance criteria related to non-functional requirements are shown in italic. Table 2 shows all (current) CineMatch User Stories related to non-functional requirements. Their acceptance criteria give an idea of how it will be tested, more about that in the section about **[Test Methodologies](#5-test-methodologies)**.

**Table 1**

_All Stories Sorted through the MoSCoW-method with the corresponding acceptance criteria_
| ID | Actor | User Story | Priority | Acceptance Criteria |
|----|-------|------------|----------|---------------------|
| US-01 | User | As a user, I want to match with another user based on my movie preferences and schedule so I don't have to be by myself in the theatre and meet new people. | M | <ul> <li>The user should be able to enter one or more of the following preferences: movie(s), movie genre(s), schedule (available time slot or date), max distance and location.</li><li>The system should suggest matches with other users who have similar preferences and are available to see the movie at the same time and location.</li><li>The user should not be able to set their schedule in the past.</li><li>The user should be able to enter their preferred method of communication with a potential match, such as messaging or email.</li><li>The user should be able to edit their preferences at all times.</li></ul>
| US-02 | User | As a user, I want to be able to select a movie from a list of available movies after entering my preferred cinema so that it is much easier and faster to decide on what I want to watch. | M | <ul><li>The system should display a list of available movies for the selected cinema.</li> <li>The user should be able to select a movie from the list of available movies.</li><li> The system should prevent the user from selecting a movie is not available at the selected cinema.</li><li> The user should be able to view more information about a movie by clicking on it in the list, such as a synopsis or trailer. </li><li> The user should be able to see the rating or reviews for a movie in the list. </li></ul>
| US-03 | User | As a user, I want to see an overview of all other users so I can find a match by myself. | M | <ul><li>The system should display a list of all other users.</li> <li>The list should include details such as the name of the user, and preferences like the movie, movie genre and the location or cinema.</li><li>The user should be able to click on a match to view more details.</li><li> The user should be able to show interest in another user.</li><li>The system should not include the active user in the overview.</li></ul>
| US-04 | User | As a user, I want to be able to sign up using my social media accounts to save time and effort in creating a new account. | M | <ul><li>The login page should offer the option to sign up using one or more supported social media platforms.</li><li>The system should display a confirmation message indicating that the user has successfully signed up using their social media account.</li><li>The user should be able to access the platform using their social media credentials in the future, without needing to sign up or remember a separate set of login credentials.</li><li>The system should store the necessary user information obtained from the social media platform, such as their name and email address.</li><li>The user should be able to choose which information to share from their social media account, such as their profile picture or other basic details.</li><li>The system should verify that the user's social media account is valid and active before allowing them to sign up.</li><li>The system should handle errors gracefully, displaying helpful messages if the social media account is not recognized or there is a problem with the authentication process.</li></ul> |
| US-05 | User | As a user, I want to be able to make a decision on whether to connect with a match or not, so I can decide if I want to pursue the match further. | M |<ul><li> The user should be able to like or dislike the proposed match. </li><li>The system should remember the decision of the user regarding a match.</li><li>The user should be able to view the other user's movie preferences and schedule.</li><li>The system should not show a user again if the response of the active user was negative.</li><li>The interface should be simple, clear and easy to use.</li></ul>
| US-06 | User | As a user, I want to be matched with different users each time I use CineMatch so that I can meet new people and avoid matching with the same user repeatedly. | S | <ul><li>The system should keep track of previously matched users and avoid suggesting them again to the user in case of a negative response.</li><li>The system should not show users that already have an existing match with the active user</li><li>The current user should be able to view the profile of the active user before making a decision.</li></ul>  |
| US-07 | User | As a user, I want to know what movies are currently playing in theaters so that I can decide which movie to watch. | S | <ul><li>The system should provide users with the ability to report inaccuracies in the movie list and offer a feedback mechanism to improve data quality.</li><li>The system should display a list of movies based on the selected cinema.</li><li>The system should remove movies from the list if it is confirmed they are no longer playing in theaters.</li><li>The list should include details such as the movie title, genre, rating, and runtime.</li><li>The user should be able to click on a movie to view more details about it.</li></ul> |
| US-08 | User | As a user, I want the ability to filter the list of currently playing movies in theaters, so that I can quickly find movies that match my preferences and interests. | S | <ul><li>The filter options should use commonly recognized movie classifications, such as MPAA rating, genre categories, and runtime categories.</li><li>The system should display the number of results that match the selected filter criteria.</li><li>The filter options should be clearly labeled and easy to use.</li><li>The user should be able to select multiple filter options at the same time.</li><li>The system should default to no filters applied during a first visit.</li><li>The user should be able to easily clear all filters applied to the movie list.</li></ul>
| US-09 | User | As a user, I want to be able to share my experience with the matches so others know what to expect when they use the app. | S | <ul><li>The user should be able to rate their matching experience.</li><li>The user should be able to write a review about their match experience.</li><li>The review should be limited to a certain number of characters.</li><li>The user should be able to edit their review after submission.</li><li>The user should be able to delete their review.</li></ul> |
| US-10 | User | As a user, I want to be able to read reviews from other users so I can feel more comfortable with using the app. | S | <ul><li>The reviews should be visible to other users.</li><li>The reviews should be ordered by date and time of submission.</li><li>The reviews should include author's name.</li><li>The reviews should display the author's rating alongside their written review.</li><li>The user should be able to report inappropriate or fake reviews for moderation.</li></ul>
| US-11 | User | As a user, I want to be able to tag the person I went to the cinema with in my review, so they can see my feedback and we can share our experience. | C | <ul><li>The user should be able to tag their match in their review.</li><li>The tagged match should receive a notification when they are tagged in a review.</li><li>The tagged match should be able to accept or decline the tag.</li><li>If the tagged match accepts the tag, their username or display name should be included in the review.</li><li>If the tagged match declines the tag, their username or display name should not be included in the review.</li></ul>
| US-12 | User | As a user, I want to be able to keep a ranked list of movies I want to watch so it will be easy for me to pick the next movie to watch. | C | <ul><li>The user should be able to add movies to their ranked list.</li><li>The user should be able to remove movies from their ranked list.</li><li>The user should be able to reorder the movies on their ranked list.</li><li>The user should be able to search for movies to add to their ranked list.</li><li>The user should be able to sort their ranked list by various criteria, such as release date, rating, or genre.</li><li>The user should be able to mark movies as watched or unwatched in their ranked list.</li></ul>
 
**Table 2**
| ID | Story | Priority | Acceptance Criteria |
| --- | --- | --- | --- | 
| NFR-US-01 | As a user, I want to easily find my way back home so that I do not have to spend my time being lost on the web application. | M | <ul><li>The home button should be easily visible on every page of the application, preferably in the same location on each page.</li><li> User feedback should be gathered to ensure that the home page and home button meet their needs and expectations.</li><li>The home page should be designed in a way that is visually appealing and easy to navigate.</li><li>There must be some way for the user to tell which steps they took and how they got where they are now.</li><li>The user must be able to visually tell where they are in the application.</li></ul> | 
| NFR-US-02 | As a user, I want each page in the application to load within 2 seconds so that I can use the application quickly and don't have to wait. | M | <ul><li>Each page in the application should load the most important attributes, within 2 seconds.</li><li>Page loading times should be measured and monitored regularly to ensure that they continue to meet the 2 second requirement.</li> <li>If a page takes longer than 2 seconds to load, the user should be presented with a loading indicator or progress bar to indicate that the system is working and prevent the user from thinking that the application is frozen or unresponsive.</li> <li> If a page consistently takes longer than 2 seconds to load, the development team should investigate the issue and make necessary changes to improve performance.</li> </ul>| 
| NFR-US-03 | As an administrator, I want the system to be able to handle a large number of users at the same time without slowing down or crashing so that all the users have a consistent experience. | M | <ul><li>The response time for all user actions should not exceed 5 seconds, even under heavy load.</li><li>Automated load testing should be performed regularly to ensure the system is meeting the performance requirements.</li><li>The performance testing should be carried out on different environments, including development, staging, and production, to ensure consistent results.</li><li>The system should be designed to handle different types of requests, with equal efficiency. </li></ul> |
| NFR-US-04 | As a user, I want to be confident that my personal and financial information is kept safe while using the application. | M | <ul><li>The application must have appropriate access controls in place to ensure that users only have access to the information they are authorized to view. </li><li> The application must never store plain text passwords in the database. </li><li> The application must have mechanisms in place to protect against common security vulnerabilities such as SQL injection.</li> <li> The application should use secure and up-to-date protocols for SSO authentication. </li> <li> The application should periodically review and update its SSO authentication system to ensure it remains secure and effective. </li></ul>
| NFR-US-05 | As a user, I want the application to be compatible with the latest versions of popular web browsers (such as Chrome, Firefox, and Safari) so that I can use the application on the browser of my choice. | M | <ul><li>The CineMatch application should be responsive and adjust to different device types, including desktop, tablet, and mobile devices. </li><li>The CineMatch application should not display any broken images on any of the supported web browsers. </li><li> The application should not show any distorted text or graphics on any of the supported web browsers.</li><li> The application should load and function properly without any issues or delays on any of the supported web browsers. </li><li>The application should be tested on the latest version of the web browser and any necessary updates or patches should be made to ensure full compatibility. </li></ul> |


### 3.1 Definition of Done
1.  The Story meets all the acceptance criteria.
2.  The code behind the Story has been reviewed by a senior programmer.
3.  The code has been optimized for readability and maintainability, using appropriate coding standards and best practices.
4.  The code has been documented to a sufficient level of detail to allow other team members to understand and modify the code if necessary.
5.  Automated tests have been created and integrated with the CI/CD pipeline, and have passed successfully.
6.  The Story can be shown through a demo.
7.  The Story has been reviewed and approved by the product owner or other relevant stakeholders, and any feedback has been incorporated into the final implementation.
8.  Any relevant documentation (e.g. release notes, user guides, technical specifications) have been updated to reflect the changes made as part of the Story.



## 4. User Interface Sketches

The following four wireframes are an initial representation of CineMatch's design. For a more complete picture, please see the prototype present in the **[Web Application in Practice](WebApplication_InPractice.md#6-user-experience)** document, which includes high-fidelity screens and more detailed information.

When I started the project, I spent time considering the user flow and created these wireframes accordingly to explore different design options and refine the user experience. I drew inspiration for these designs from other cinema-based and dating websites found on dribbble. The first sketches were done on paper and later converted to images using the application Figma. The primary goal was to ensure the features were clear and concise, with easy access to frequently used functions (such as the left menu), while allowing enough space to avoid clutter and enhance the user experience.

### 4.1 Homepage (3 versions)

To choose the best design for the homepage, I conducted a survey on social media for homepage designs found on dribbble. Also more about that process in the **[Web Application in Practice](WebApplication_InPractice.md#6-user-experience)** document. I made a top three out of these choices.

The first two designs both received six votes. The third version can be seen as a combination of the other two, but it was initially inspired by a different design I had come across.

1. Design 1: <img src="/Media/Homepage_version1.png" alt="Homepage of CineMatch, version 1" width="800">
	1. Participants liked the clear view of this design, which uses a three-column layout to make it easy to focus on the most important elements. The first column features a minimalist menu with labels that are turned 90 degrees, giving it a unique and modern look. Despite the unusual orientation, users appreciated that it didn't take up too much space on the page. The third column of the design features the media, which could include a large image of a movie poster or something cinema related to grab the user's attention. This visual element helps users quickly understand what the service is about and what they can expect to see. Overall, the design strikes a balance between simplicity, a modern look and functionality, making it easy to use while still providing all the necessary information. The results of the survey suggest that end-users are open to different design approaches and are willing to embrace new ideas.

2. Design 2: <img src="/Media/Homepage_version_2.png" alt="Homepage of CineMatch, version 2" width="800">
	1. Participants liked this design because of the aesthetic and its amount of space. In addition, I've received comments about it looking more trustworthy. The design that inspired this version had two drawings next to the "Get Started" button. No clutter and very inviting towards the audience. Users expressed a desire for a visually appealing background that would create a welcoming and comfortable atmosphere on the website, particularly around the central button, as this is where their focus lies. It's worth noting that this design was favored by participants with an artistic background, perhaps indicating a preference for aesthetics and visual appeal.

3. Design 3 (3 votes): <img src="/Media/Homepage_version_3.png" alt="Homepage of CineMatch, combination of the other two" width="800">
	1. Participants also liked the clear view of this design. The two-column design made it easy on what to focus on, accompanied by the classic top menu. It didn't overly complicate things, which is one of the main things to keep in mind when it comes to user experience. Interestingly, the third design appears to be a blend of the other two, featuring the top menu from design 2 and the media placement on the right from the first design.

### 4.2 Preferences
The preferences screen allows users to set their movie and genre preferences, as well as date, timeslot, maximum travel distance, and location. By providing this information, the system can better match users with compatible movie buddies based on their interests and schedules.

In the future, we plan to add an additional field where users can select their preferred cinema based on their location. Once a movie is selected, a thumbnail of the movie will appear next to the form. If no movie has been selected yet, the thumbnail area will remain empty. To make it easier for users, there are two lists available: one that displays currently showing movies (if a cinema has been selected), and one that shows upcoming movies that are not yet showing anywhere.

Overall, the preferences screen provides users with the ability to tailor their matches to their schedule and movie interests, increasing the chances of finding a compatible movie buddy.


<img src="/Media/Preferences_screen.png" alt="Setting preferences screen" width="800">

### 4.3 Matches overview
After logging in, users will be presented with a selection of potential matches, allowing them to quickly find a match and simplify the process. The idea is to show the poster of a selected movie or something representing a genre, alongside the other user's name. The active user can then read more about one of the other users by clicking on their respective card. 

<img src="/Media/Matching_screen.png" alt="Homepage of CineMatch after logging in, showing potential matches" width="800">

### 4.4 Match browsing
After selecting the CTA button "Find a Match", users will see this screen. It has yet to be decided whether multiple cards will be visible at the same time or just one, and how much information will be shown. However, the other user's name, profile picture, and movie or genre selection will be definitely be displayed, along with a button for the active user to show interest in the other user.

<img src="/Media/Match_browsing_v1_1.png" alt="Browsing through matches" width="800">


## 5. Test Methodologies
Due to the size of the quality plan, there is a separate document that covers the test methodologies: **[Quality plan](quality_plan.md)**. This document details the various testing approaches we are utilizing throughout the software development lifecycle, including unit testing, integration testing, system testing, and acceptance testing. It also describes the specific tools and frameworks I used to automate the testing processes and ensure the quality and reliability of CineMatch. Usability testing is covered under the User Experience section in the **[Web Application in Practice](WebApplication_InPractice.md#6-user-experience)** document.
