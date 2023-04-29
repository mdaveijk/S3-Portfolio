# CineMatch Project Analysis
<!-- 
![CineMatch logo|200](/Media/cinematchlogo.png)
-->
<img src="/Media/cinematchlogo.png" alt="CineMatch logo" width="200">

___
|         |         |
|:---------------:|:---------------:|
|   Release:      |   28th of April 2023      |
|   Version:          |   2.0          |
|   Location:          |   Eindhoven          |
|   Project owner:          |   Maurice Schippers          |
___


## Table of Contents
- [1. Project Description](#1-project-description)
- [2. System Architecture Overview](#2-system-architecture-overview)
	- [2.1 Components](21-cinematch-components)
	- [2.2 The C4-model](#22-the-c4-model)
		- [2.2.1 System Context](221-system-context-diagram)
- [3. User Stories and Quality Measurement](#3-user-stories-and-quality-measurement)
	- [3.1 Definition of Done](#31-definition-of-done)
- [4. User Interface Sketches](#4-user-interface-sketches)
	- [4.1 Homepage](#41-homepage-3-versions)
	- [4.2 Preferences](42-preferences)
	- [4.3 Matches overview](43-matches-overview)
	- [4.4 Match browsing](44-match-browsing)
- [5. Test Methodologies](#5-test-methodologies)

## 1. Project Description
CineMatch is a software project that allows users to plan a visit or **movie meetup** to a certain movie, within a certain timeslot and location and let the software find a match with another user that has scheduled for a matching or similar movie (genre), timeslot or location. Once a match has been made, users can communicate through the application to decide on further details and leave a review about their overall experience afterwards. 
While similar in some respects to popular dating apps, such as Tinder, CineMatch is intended to serve a different purpose.

The need for a ‘buddy’ to do activities with is high nowadays. Especially after COVID-19, people are struggling to be more social and finding an activity within a shared interest can be hard. This initiative seeks to alleviate social isolation and encourage social interaction by enabling users to connect with others interested in going to the movies. There are currently no (popular) projects like this, except for a few that have a broad range of activities and not just a cinema visit. This might give users the opportunity to have an app to go to, a central place, when they do not want to be by themselves in the cinema. The project's ultimate goal is to expand beyond the cinema and encompass a broader range of activities.


## 2. System Architecture Overview

### 2.1 Components

### 2.2 The C4-model
The C4-model depicts a visual representation of the architecture of CineMatch. Making it easier to understand and communicate for both technical and non-technical stakeholders.

#### 2.2.1 System Context Diagram
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
| US-04 | User | As a user, I want to be able to sign up using my social media accounts to save time and effort in creating a new account. | M | <ul><li>The login page should offer the option to sign up using one or more supported social media platforms.</li><li>The system should display a confirmation message indicating that the user has successfully signed up using their social media account.</li><li>The user should be able to access the platform using their social media credentials in the future, without needing to sign up or remember a separate set of login credentials.</li><li>The system should store the necessary user information obtained from the social media platform, such as their name and email address.</li><li>The user should be able to choose which information to share from their social media account, such as their profile picture or other basic details.</li><li>The system should verify that the user's social media account is valid and active before allowing them to sign up.</li><li>The system should handle errors gracefully, displaying helpful messages if the social media account is not recognized or there is a problem with the authentication process.</li></ul> |
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
In addition to the project analysis outlining the design choices and requirements of our software application, there is a separate document that covers our test methodologies. This document details the various testing approaches we are utilizing throughout the software development lifecycle, including unit testing, integration testing, system testing, and acceptance testing. It also describes the specific tools and frameworks we are using to automate our testing processes and ensure the quality and reliability of our software product. By following these rigorous testing procedures, we hope to deliver a high-performing, bug-free application that meets the needs and expectations of our stakeholders.
