# CineMatch Project Analysis
<!-- 
![CineMatch logo|200](/Media/cinematchlogo.png)
-->
<img src="/Media/cinematchlogo.png" alt="CineMatch logo" width="200">


## Table of Contents
1. [Project Description](#1-project-description)
2. [C4-Model: Architecure of the application](#2-c4-model-architecture-of-the-application)
	1. [System Context](#21-system-context-diagram)
3. [User Stories and Quality Measurement](#3-user-stories-and-quality-measurement)
	1. [Definition of Done](#31-definition-of-done)
4. [User Interface Sketches](#4-user-interface-sketches)
	1. [Homepage](#41-homepage)
	2. [Schedule a visit](#42-schedule-a-visit)
	3. [Schedule a visit - Possible matches](#43-schedule-a-visit---possible-matches)
	4. [Movie meetup overview page](#44-movie-meetup-overview-page)
5. [Test Methodologies](#5-test-methodologies)

## 1. Project Description
CineMatch is a software project that allows users to plan a visit or **movie meetup** to a certain movie, within a certain timeslot and location and let the software find a match with another user that has scheduled for a matching or similar movie (genre), timeslot or location. Once a match has been made, users can communicate through the application to decide on further details and leave a review about their overall experience afterwards. 
While similar in some respects to popular dating apps, such as Tinder, CineMatch is intended to serve a different purpose.

The need for a ‘buddy’ to do activities with is high nowadays. Especially after COVID-19, people are struggling to be more social and finding an activity within a shared interest can be hard. This initiative seeks to alleviate social isolation and encourage social interaction by enabling users to connect with others interested in going to the movies. There are currently no (popular) projects like this, except for a few that have a broad range of activities and not just a cinema visit. This might give users the opportunity to have an app to go to, a central place, when they do not want to be by themselves in the cinema. The project's ultimate goal is to expand beyond the cinema and encompass a broader range of activities.


## 2. C4-Model: Architecture of the application
The C4-model depicts a visual representation of the architecture of CineMatch. Making it easier to understand and communicate for both technical and non-technical stakeholders.

#### 2.1 System Context Diagram
<img src="/Media/SystemContext_CineMatch.png" alt="CineMatch Context Diagram" width="600">
The social cinemagoer is the primary stakeholder for this project, and their satisfaction is a top priority. To streamline the development process and improve user experience, CineMatch makes use of two separate APIs. The first API handles user authorization and authentication, enabling users to sign up using their social media accounts. The second API is the IMDB API, which provides access to the latest movie information and additional data, including images. 

<!-- ![SystemContext_CineMatch.png](/Media/SystemContext_CineMatch.png) -->

## 3. User Stories and Quality Measurement

Table 1 shows all (current) CineMatch User Stories sorted by priority.

**Table 1**

_All Stories Sorted through the MoSCoW-method with the corresponding acceptance criteria_
| ID | Actor | User Story | Priority | Acceptance Criteria |
|----|-------|------------|----------|---------------------|
| US-01 | User | As a user, I want to match with another user based on my movie preferences and schedule so I don't have to be by myself in the theatre and meet new people. | M | <ul> <li>The user should be able to select one or more of the following criteria for matching: movie, movie genre, available time slot, date, and location.</li> <li> The user has to enter at least one matching criteria. </li> <li>The system should suggest matches with other users who have similar preferences and are available to see the movie at the same time and location.</li> <li>The user should be able to specify the maximum distance they are willing to travel to the cinema.</li> <li>The user should not be able to schedule a visit in the past.</li> <li> The user is allowed to cancel a movie meetup if they haven't been matched yet.</li> <li> The user can schedule multiple visits. </li> </ul>
| US-02 | User | As a user, I want to be able to select a movie from a list of available movies after entering my preferred cinema so that it is much easier and faster to decide on what I want to watch. | M | <ul><li>The system should display a list of available movies for the selected cinema.</li> <li>The user should be able to select a movie from the list of available movies.</li><li> The system should prevent the user from selecting a movie is not available at the selected cinema.</li><li> The user should be able to view more information about a movie by clicking on it in the list, such as a synopsis or trailer. </li><li> The user should be able to see the rating or reviews for a movie in the list. </li></ul>
| US-03 | User | As a user, I want to see an overview from all open movie meetups by other users so I can find a match by myself. | M | <ul><li>The system should display a list of all open movie meetups by other users.</li> <li>The list should include details such as the user who scheduled the meetup, the movie, date and time, and location of the meetup.</li><li>The system should not show meetups that already found a match or are already full.</li><li>The user should be able to click on a movie meetup to view more details about it.</li><li> The user should be able to apply for an open movie meetup scheduled by another user. </li><li>The system should remove movie meetups that have passed.</li></ul>
| US-04 | Admin | As a website administrator, I want users to create an account to improve the security of the application and protect user data. | M | <ul><li>The system should display a registration form for users to create an account.</li><li>The registration form should require the user's full name, email address and a password.</li><li>The system should verify that the email address is unique and not already registered.</li><li>The system should encrypt and store the user's password securely.</li><li>The system should display an error message if the user enters an invalid email address or password.</li><li>The user should be able to log in to their account using their email address and password.</li></ul> |
| US-05 | User | As a user, I want the ability to limit the number of times another user can apply for my movie meetup, so that I don't receive spam requests and can focus on finding the right match for my movie visit. | M |<ul><li>The system should restrict the amount of times another user can sign up for a certain movie meetup.</li><li>The system should prevent users from applying for a meetup once the limit has been reached.</li><li>The system should notify the user when a user's application for their movie meetup has been declined due to the limit being reached.</li><li>The system should keep track of the number of times another user has applied for a user's movie meetup and prevent them from applying once the limit has been reached.</li><li>The system should prevent a user from signing up for the same movie meetup more than once.</li><li>The system should display the number of remaining available spots for a movie meetup to other users.</li></ul>
| US-06 | User | As a user, I want to decide whether a match works for me so I won't get matched with the same person every time. | S | <ul><li>The system should allow another user to apply for a match with the current user.</li><li>The current user should be able to accept or decline the match request.</li><li>If the current user declines a match request, the system should suggest a new user who is interested in a match.</li><li>The current user should be able to view the profile of the user who has applied for a match before accepting or declining.</li><li>The current user should be able to set preferences for the types of matches they want to receive.</li><li>The system should take into account the current user's match preferences when suggesting a new user.</li><li>The current user should be able to provide feedback on the match request to improve future matches.</li></ul>  |
| US-07 | User | As a user, I want to know what movies are currently playing in theaters so that I can decide which movie to watch. | S | <ul><li>The system should provide users with the ability to report inaccuracies in the movie list and offer a feedback mechanism to improve data quality.</li><li>The system should display a list of movies based on the selected cinema.</li><li>The system should remove movies from the list if it is confirmed they are no longer playing in theaters.</li><li>The list should include details such as the movie title, genre, rating, and runtime.</li><li>The user should be able to click on a movie to view more details about it.</li></ul> |
| US-08 | User | As a user, I want the ability to filter the list of currently playing movies in theaters, so that I can quickly find movies that match my preferences and interests. | S | <ul><li>The filter options should use commonly recognized movie classifications, such as MPAA rating, genre categories, and runtime categories.</li><li>The system should display the number of results that match the selected filter criteria.</li><li>The filter options should be clearly labeled and easy to use.</li><li>The user should be able to select multiple filter options at the same time.</li><li>The system should default to no filters applied when the user first navigates to the movies page.</li><li>The user should be able to easily clear all filters applied to the movie list.</li></ul>
| US-09 | User | As a user, I want to be able to share my experience with the movie meetup so others know what to expect when they use the app. | S | <ul><li>The user should be able to rate their movie meetup experience.</li><li>The user should be able to write a review about their movie meetup experience.</li><li>The review should be limited to a certain number of characters.</li><li>The user should be able to edit their review after submission.</li><li>The user should be able to delete their review.</li></ul> |
| US-10 | User | As a user, I want to be able to read reviews from other users so I can feel more comfortable with using the app. | S | <ul><li>The reviews should be visible to other users.</li><li>The reviews should be ordered by date and time of submission.</li><li>The reviews should include author's name.</li><li>The reviews should display the author's rating alongside their written review.</li><li>The user should be able to report inappropriate or fake reviews for moderation.</li></ul>
| US-11 | User | As a user, I want to be able to tag the person I went to the cinema with in my review, so they can see my feedback and we can share our experience. | C | <ul><li>The user should be able to tag their match in their review.</li><li>The tagged match should receive a notification when they are tagged in a review.</li><li>The tagged match should be able to accept or decline the tag.</li><li>If the tagged match accepts the tag, their username or display name should be included in the review.</li><li>If the tagged match declines the tag, their username or display name should not be included in the review.</li></ul>
| US-12 | User | As a user, I want to be able to keep a ranked list of movies I want to watch so it will be easy for me to pick the next movie to watch. | C | <ul><li>The user should be able to add movies to their ranked list.</li><li>The user should be able to remove movies from their ranked list.</li><li>The user should be able to reorder the movies on their ranked list.</li><li>The user should be able to search for movies to add to their ranked list.</li><li>The user should be able to sort their ranked list by various criteria, such as release date, rating, or genre.</li><li>The user should be able to mark movies as watched or unwatched in their ranked list.</li></ul> |



### 3.1 Definition of Done
1.  The Story meets all the acceptance criteria.
2.  The code behind the Story has been reviewed by a senior programmer.
3.  The code has been optimized for readability and maintainability, using appropriate coding standards and best practices.
4.  The code has been documented to a sufficient level of detail to allow other team members to understand and modify the code if necessary.
5.  Automated tests have been created and integrated with the CI/CD pipeline, and have passed successfully.
6.  The Story can be shown through a demo.
7.  The Story has been reviewed and approved by the product owner or other relevant stakeholders, and any feedback has been incorporated into the final implementation.
8.  Any relevant documentation (e.g. release notes, user guides, technical specifications) have been updated to reflect the changes made as part of the Story.

<!-- 
| 1 | User | Create an account | M | User is able to create a new account with a unique username and password. |
| 2 | User | Log in and out | M | User is able to log in to their existing account and log out when they are finished. |
| 3 | User | Search for movies and showtimes | M | User is able to search for movies and see their showtimes. |
| 4 | User | Schedule a cinema visit | M | User is able to schedule a cinema visit by selecting a movie and showtime. |
| 5 | User | Match with other users | M | User is matched with other users who have similar movie preferences, showtimes, and locations. |
| 6 | User | Chat with match | S | User is able to chat with their match after a successful match. |
| 7 | User | See movie and cinema reviews | S | User is able to see reviews of movies and cinema locations. |
| 8 | User | Leave a review | S | User is able to leave a review after watching a movie. |
| 9 | User | Keep a "to watch" list | S | User is able to keep a list of movies they want to watch. |
| 10 | User | Follow another user | S | User is able to follow another user and see their activity. |
| 11 | User | Filter matches by criteria | S | User is able to filter their matches by certain criteria, such as age or gender. |
| 12 | User | User-friendly application | M | The application is easy to navigate and use. |
| 13 | User | Responsive application | M | The application works well on both desktop and mobile devices. |
| 14 | System | Fast response time | M | The application has a fast response time and minimal downtime. |
| 15 | System | Secure user data | M | The application is secure and protects user data. | -->



## 4. User Interface Sketches
Below are four wireframes to give an idea of the main design of CineMatch. The inspiration for these designs came from other cinema-based websites found on dribbble. The first sketches were done on paper and later converted to images using the application Figma. The primary goal was to ensure the features were clear and concise, with easy access to frequently used functions (such as the upper right buttons and left menu), while allowing enough space to avoid clutter and enhance the user experience.

### 4.1 Homepage
<img src="/Media/Homepage.png" alt="Homepage of CineMatch" width="800">
The homepage features a number of elements to provide an easy, yet not too busy interface. On the left of the page, there is a block that features a small menu, a section that features the name of friends made through matches displayed in a vertical order and a log out button. This block will be available on every page. The same goes for the top bar that features a CTA button to organise a movie meetup and two buttons for notifications and to access their personal account. Unique to the page are the main content area that will feature a welcome message and tell the user basic information about CineMatch. Additionally, a sign up button is shown in case the user does not have an account yet. Finally, the page has a list of the (upcoming) meetups that the user is part of. 

### 4.2 Schedule a visit
<img src="/Media/ScheduleAVisit.png" alt="Scheduling a visit - CineMatch" width="800">
This is the view the user sees when they click the "Let's go to the movies" CTA button. Here, they can submit their preferences when looking for a match, such as their preferred movie, movie genre, location, and more. Once they have selected a movie, a thumbnail of the movie will show up next to the form. If no movie has been selected yet, the thumbnail area will remain empty. To make it easier for the user, there are two lists available: one that displays the movies currently showing (if the user has selected a cinema), and one that shows upcoming movies that are not yet showing anywhere.

### 4.3 Schedule a visit - Possible matches
<img src="/Media/ScheduleAVisitPossibleMatches.png" alt="Scheduling a visit with possible matches - CineMatch" width="800">
This is the view that the user sees after submitting the form to schedule a visit. The system displays any possible matches based on the user's preferences. Each match shows the avatar of the matched user, their name, the matching information (for example, preferred movie, movie genre, location), and the preferred date and time slot. The matching information will be shown in bold text to help the user quickly identify key details. If the user is interested in a suggested match, they can connect with the user by selecting the "Confirm match" button.

### 4.4 Movie meetup overview page
<img src="/Media/MovieMeetupOverview.png" alt="Overview of all open movie meetups - CineMatch" width="800">
This view displays a complete overview of all currently available movie meetups that the user can choose from. Each meetup shows the avatar of the organiser, their name, and the details of the meetup, which could be the movie, a genre, location or a preferred date and time. It also shows the current status of the meetup, such as whether it's open to join or already full. If the active user is interested in the meetup, they can connect with the organiser by selecting the "Show interest" button.

## 5. Test Methodologies
In addition to the project analysis outlining the design choices and requirements of our software application, there is a separate document that covers our test methodologies. This document details the various testing approaches we are utilizing throughout the software development lifecycle, including unit testing, integration testing, system testing, and acceptance testing. It also describes the specific tools and frameworks we are using to automate our testing processes and ensure the quality and reliability of our software product. By following these rigorous testing procedures, we hope to deliver a high-performing, bug-free application that meets the needs and expectations of our stakeholders.
