# How can we effectively find and prevent identification and authentication failures in a distributed web application?


## Table of Contents
- [1. Introduction](#1-introduction)
- [2. Understanding Identification and Authentication in (Distributed) Web Applications](#2-understanding-identification-and-authentication-in-distributed-web-applications)
	- [2.1 A Real-Life Comparison](#21-a-real-life-comparison)
	- [2.2 In the Context of Web Applications](#22-in-the-context-of-web-applications)
- [3. Problem Analysis](#3-problem-analysis)
	- [3.1 What causes authentication failures?](#31-what-causes-authentication-failures)
	- [3.2 Why do authentication failures continue to occur?](#32-why-do-authentication-failures-continue-to-occur)
- [4. What can we do to prevent identification and authentication failures?](#4-what-can-we-do-to-prevent-identification-and-authentication-failures)
- [5. Possible solution: Using a Single Sign-On Service](#5-possible-solution-using-a-single-sign-on-service)
	- [5.1 The Benefits of SSO](#51-the-benefits-of-sso)
	- [5.2 Keycloak](#52-keycloak)
- [6. Testing the Security of Keycloak](#6-testing-the-security-of-keycloak)
	- [6.1 Credential Stuffing attack](#61-credential-stuffing-attack)
	- [6.2 Exploring Keycloak's Additional Security Features](#62-exploring-keycloaks-additional-security-features)
- [7. Conclusion](#7-conclusion)
- [8. References](#8-references)

## 1. Introduction

With the ever-increasing reliance on technology in our everyday lives, the importance of secure practices in software design is more critical than ever. Sensitive personal information and other valuable data is stored and transferred digitally and should be kept safe through any necessary measures. To find out how I can best protect my own application, I decided to make this into a full research report.

Unfortunately, we are all too familiar with the amount of data breaches reported on the news. These incidents highlight the vulnerabilities and the potential consequences of weak security measures. One of the ways hackers are able to exploit these weaknesses, is through <u>identification and authentication failures</u>, which, as of the time of writing, ranks 7th on the OWASP top 10 list of security vulnerabilities. (_A07 Identification and Authentication Failures - OWASP Top 10:2021_, n.d.)

So, this begs the question: **How can we effectively find and prevent identification and authentication failures in a distributed web application?** This is the main focus of this report, aiming to provide practical solutions and insights as an answer. Considering this is a very broad subject, this research will focus specifically on identification and authentication failures within the context of a distributed web application.

In order to achieve this, I have used a number of DOT-framework methods to gather my information:
1. Root cause analysis (Workshop)
2. Best, good and bad practices (Library)
3. Available product analysis (Library)
4. Prototyping (Workshop)
5. Security test (Lab)

Please note that I used the prototype to conduct the security tests on. 
 
## 2. Understanding Identification and Authentication in (Distributed) Web Applications

Since identification and authentication is a broad topic, it is important to first establish a mutual understanding of the subject. This section aims to provide some clarity on what it is all about. 

### 2.1 A Real-Life Comparison
Identification and authentication is actually a two-step process. Imagine you bought a ticket to a concert by your favourite artist. You show your ticket to the staff member, which is registered on your name and contains some other information. That is the first step of the process: **identification**. It states who you claim to be and provides some initial information about you. However, simply showing a ticket may not be sufficient to gain entry. This is where the second step, **authentication**, comes into play. Authentication is like a verification process that ensures the ticket belongs to you and that you meet the necessary criteria to attend the concert. It involves additional checks, such as comparing your ID with the name on the ticket or validating your age if it's an age-restricted event. 

### 2.2 In the Context of Web Applications

Similarly, in the world of web applications, identification and authentication serve as a kind of "gatekeepers" for granting access to authorized users and protecting sensitive information. Just as a concert organizer wants to ensure only legitimate ticket holders can enter the venue, web applications need to verify the identity of users before granting them access to protected resources. The best example of this is that most websites require you to sign up in order to use most of the functionalities the website offers. After you sign up (identification), you usually receive an e-mail to confirm your identity (authentication). (_Identification & Authentication: Similarities & Differences | Okta_, n.d.)

Gaining unauthorized access to a concert might not result in the worst possible scenario. But, imagine the consequences when someone with bad intentions successfully attempts to impersonate a high-ranking official who holds the key to millions of files containing sensitive information. This is essentially what a hacker does—exploiting vulnerabilities to gain unauthorized access and compromise systems. In the next section, we will explore the specific vulnerabilities that hackers exploit and delve deeper into the underlying causes of these failures.

## 3. Problem Analysis

Now we understand the concepts, it is time to dig deeper and find the root causes of common authentication failures.

### 3.1 What causes authentication failures?

The following are some common vulnerabilities identified by OWASP that can lead to unauthorized access and compromise the security of web applications (OWASP Top 10:2021, n.d.):

1. **Automated Attacks**: Attackers exploit weak or obvious user credentials, lack of account lockout mechanisms (like a timeout after attempting to login too many times), or poor protection against brute-force attacks, such as credential stuffing, where attackers use a list of known usernames and passwords to gain unauthorized access.
2. **Weak Passwords**: Common, weak, or easily guessable passwords pose a huge threat. Weak password policies, the lack of password complexity requirements and ineffective password management contribute to this vulnerability. 
3. **Ineffective Credential Recovery**: Insecure credential recovery and forgot-password processes can be exploited, if they rely on insecure methods such as knowledge-based answers that are easily guessable or publicly available.
4. **Insecure Storage of Passwords**: Storing passwords in plain text, encrypted form, or with weak hashing algorithms makes it easy for attackers to gain access to the system.
5. **Inadequate Multi-Factor Authentication (MFA)**: Multi-factor authentication adds an extra layer of security by requiring users to provide additional verification factors beyond just a password. Absence or weak implementation of MFA leaves web applications vulnerable to attacks, even if the user's password is strong. Fontys University recently implemented this through the use of Microsoft's Multifactor Authentication smartphone app.
6. **Session Management Issues**: Flaws in session management, such as exposing session identifiers in URLs or reusing session identifiers, and failing to properly invalidate session IDs during logout or periods of inactivity are common vulnerabilities. Attackers can exploit these weaknesses to impersonate real users and perform unauthorized actions.

This list serves as a valuable resource for finding security issues within our web applications. 

### 3.2 Why do authentication failures continue to occur?

Despite the awareness and knowledge about these vulnerabilities, incidents such as data leaks and authentication problems persist. Several factors contribute to the ongoing occurrence of these issues. Below are some examples of this in practice:

- **Not securing everything**: The data leak at HAN University, where attackers exploited a webform to gain unauthorized access to sensitive information (NOS, 2021). This incident highlights the importance of properly securing web elements and conducting regular vulnerability tests to prevent such breaches. It serves as a reminder that even seemingly overlooked aspects can lead to significant security breaches.
- **User behaviour**: Another factor contributing to authentication problems is user behaviour. Research shows that many users tend to postpone or underestimate the need for additional security measures like two-factor authentication, relying solely on strong passwords (Ng, 2018). After all, the effectiveness of these measures ultimately relies on user adoption and cooperation.
- **Dependencies on third-party libraries**: Even with strong security measures in place, an application can remain vulnerable if it relies on third-party libraries with security issues. An example of this is these alerts I received for my GitHub frontend-project which makes use of Materialize (a design library). According to GitHub's "Dependabot", which scans for security issues, the version of Materialize that my frontend project uses is vulnerable to Cross-Site Scripting. This example shows the importance of maintaining and updating all dependencies on your application to ensure a secure software ecosystem.

  <img src="/Media/dependabot_alerts.png" alt="An example of third-party library security issues, retrieved from GitHub." width="800"> <br>
  *An example of third-party security issues, retrieved from GitHub* 

- **Reusing credentials:** Many users tend to reuse the same email address and password combination across multiple applications. This practice poses a significant risk as attackers can exploit email addresses obtained from data breaches for credential stuffing attacks. The website 'Have I Been Pwned?'[^1] is a service that allows users to check if their email addresses have been part of a data breach. Once an email address is obtained, attackers are left with the task of guessing the associated password, making brute force attacks much more likely to succeed.

These examples show that authentication failures are influenced by a combination of technical, human, and environmental factors. 

But what can we do to prevent these authentication failures and protect our applications from such threats? In the next section, we will look into effective strategies and best practices that can be used to enhance security and mitigate these risks.

___
[^1]: https://haveibeenpwned.com/

## 4. What can we do to prevent identification and authentication failures?

The OWASP article on this issue provides a list of strategies and best practices that can be implemented to protect applications (OWASP Top 10:2021, n.d.). I added a bit of extra information based on my own experience to some of these strategies. 

1. **Implement multi-factor authentication**: Utilize multi-factor authentication whenever possible to prevent automated credential stuffing, brute force, and stolen credential reuse attacks.
2. **Avoid default credentials**: Ensure that no default credentials, especially for admin users, are shipped or deployed. During the semester, I discovered that this practice extends to deployment through Docker, where the default situation sets up the container with a 'root user' that has the highest privileges and unrestricted access to the system. To reduce this risk, it is recommended to add 'another user' with lower privileges during the Docker setup process. For example, you can include the following lines in your backend Dockerfiles:

```
## To prevent the container from being run by a root user, create another user with lower privileges
RUN addgroup -S docker && adduser -S docker -G docker
USER docker:docker
```

By adding these lines, the container will be run using the 'docker' user, providing a more secure configuration.

3. **Implement weak password checks**: Include weak password checks in the application, such as testing new or changed passwords against a list of the top 10,000 worst passwords. There are several sources to use for this strategy, such as the "SecLists" repository on GitHub[^2], with exactly 10,000 passwords, or the rockyou.txt file[^3], where the latter originated from an actual data breach back in 2009 and contains over a million of passwords. 

4. **Align password policies with best practices**: Follow password length, complexity, and rotation policies that align with established guidelines, such as the National Institute of Standards and Technology (NIST) 800-63b's recommendations for Memorized Secrets. These guidelines are official documents that provide detailed instructions on how to handle passwords securely. An example of password requirements that follow these best practices can be seen below:
  <img src="/Media/password_requirements_nng.png" alt="An example of a form with password requirements that align with industry standards. Retrieved from the Nielsen Norman Group." width="800"> <br>
  *An example of a form with password requirements that align with industry standards. Retrieved from the Nielsen Norman Group.*

5. **Harden registration, credential recovery, and API pathways**: It is essential to strengthen the security of registration, credential recovery, and API pathways to mitigate the risk of account enumeration attacks and prevent information leakage. What this basically boils down to is that by refraining from providing specific error messages like "email address does not exist" or "the password is wrong", you prevent attackers from gaining insights into the validity of certain credentials. Even though this may cause some inconvenience to well-intentioned users, it is important to make it as hard as possible for attackers by holding back such information.

6. **Implement login attempt limitations and alerts**: Limit or increasingly delay failed login attempts, but be careful not to create a denial of service scenario. Log all failures and alert administrators when credential stuffing, brute force, or other attacks are detected. Regarding the warning: This is one of the more tricky ones to implement on this list. I personally recommend to use this strategy in combination with *Hardening credential recovery*, and encourage users to reset their passwords when they have one attempt left to prevent getting locked out of their account. 

7. **Use a secure session management approach**: Employ a server-side, secure, built-in session manager that generates a new random session ID with high entropy after login. Store the session identifier securely, avoid including it in URLs, and invalidate the session after logout, idle time, and absolute timeouts. The OWASP article provides a great example that highlights the importance of secure session management: "A user uses a public computer to access an application. Instead of selecting "logout," the user simply closes the browser tab and walks away. An attacker uses the same browser an hour later, and the user is still authenticated." 

So far the strategies recommended by OWASP. In addition to these, I came across another helpful article that provides additional preventive measures. Shown below is a takeaway version of the most relevant strategies recommended by tech blogger Kapoor (2022):

8. **Use web application scanners**: It is possible to use web application scanners to test your sites for various vulnerabilities, such as SQL injection or cross-site scripting (XSS). Kapoor suggests a tool like Burp Suite[^4] which offers a broader range of testing features, but may require more time to master. It is recommended to run your application through at least one type of scanner before going live. However, it is important to carefully review the scan results before taking any action, as false positives or harmless findings may happen.

9. **Set up CAPTCHA**: Implement CAPTCHA, which stands for "Completely Automated Public Turing test to tell Computers and Humans Apart" to verify that users are human, making automated input and potential attacks much harder.

10. **Regularly test your site for vulnerabilities, even during updates**: It is important to perform regular vulnerability assessments, penetration tests, and continuous testing during updates to find and patch security weaknesses in your code. Test your site's resistance against common attacks, ensure proper encryption of sensitive data, and avoid storing sensitive information in cookies: use secure database storage instead. Automated tools can help with detecting design or architecture flaws that may be overlooked during manual testing.

By using these strategies and following best practices, we can greatly reduce the risk of identification and authentication failures, enhancing the overall security of our web applications. Although the digital world is constantly evolving and new threats are on the horizon, staying proactive and following these practices is an important step in the right direction towards protecting our web applications and users. 

---
[^2]: SecLists repository on GitHub. Available at: [https://github.com/danielmiessler/SecLists](https://github.com/danielmiessler/SecLists)  More information about this at: [Wikipedia: 10,000 most common passwords](https://en.wikipedia.org/wiki/Wikipedia:10,000_most_common_passwords)
[^3]: rockyou.txt. Available at: [rockyou txt file on GitHub](https://github.com/brannondorsey/naive-hashcat/releases/download/data/rockyou.txt)
[^4]: Burp Suite. Available at: [https://portswigger.net/burp](https://portswigger.net/burp)

## 5. Possible solution: Using a Single Sign-On Service

In this section, I will explain why I decided to use a Single Sign-On (SSO) service and explain my decision to use Keycloak within my application. SSO is a mechanism that allows users to authenticate themselves once and gain access to multiple applications or systems without the need to re-enter their credentials for each individual service. By using a SSO, we are able to enhance the user experience, improve security, and simplify the authentication process for our applications. Making the process easier for both users and developers.

The idea to use SSO for my application originated from one of the user stories in CineMatch, my distributed web application. This story states that users should be able to login with their social media accounts. While looking for possible solutions, I decided to incorporate my findings into this report.

### 5.1 The Benefits of SSO

Since building a login/registration system is out of scope for this project, I decided to use a Single Sign-On (SSO) service. It can be hard to build a robust authorization and authentication system from scratch, therefore companies are more likely to delegate this responsibility to a third-party service specifically created to handle authentication and authorization. SSO services have become increasingly popular in modern applications, as they offer a reliable and secure solution for authentication and authorization. 

According to frontegg, an SSO platform offers various benefits, including (“The Advantages and Benefits of Single Sign-On (SSO),” 2023):

- Added ease of use for end-users, which enhances customer satisfaction
- Less stress on developers to create new authentication solutions
- Improved security and compliance capabilities
- A seamless experience that is easy to integrate
- Better and easier to manage, especially while scaling up fast

In addition, Okta, a leading identity management platform, highlights the significant *security* benefits of implementing SSO for applications: "Verizon’s [Data Breach Investigations Report 2021](https://www.verizon.com/business/en-gb/resources/reports/dbir/) found that 85% of breaches involved a human element, and 61% involved credentials. When you add to that the fact that in 2021 cyber attacks on corporate networks [increased 50% year over year](https://blog.checkpoint.com/2022/01/10/check-point-research-cyber-attacks-increased-50-year-over-year/), peaking at 925 attacks a week per organisation, the case for securing people’s usernames and passwords with SSO is clear." (“The Advantages and Benefits of Single Sign-On (SSO),” 2023)  

#### 5.2 Keycloak

There are a lot of SSO services out there to choose from. In order to find the right one, I decided to set out with a basic list of requirements: 

1. **Easy to use**: As a newcomer to SSO services, I looked for a solution with a low learning curve and good documentation. 
2. **Compatibility with Spring (Boot)**: Since CineMatch is built with Spring Boot, compatibility with this framework was a crucial factor.
3. **Popularity and Usage**: I considered the popularity and adoption of the SSO service to ensure its continued support and active user community.
4. **Supports social login**: Meeting the user story requirement, the SSO service should enable social media login.

After conducting some research and consulting various sources, I found that Keycloak[^5] checks all the requirements. Here's a summary of the findings:

- **Easy to use**: Keycloak offers an easy-to-use interface and is recognized for its "top-of-the-line" security features, making it an attractive option for newcomers to SSO services (Świątek, 2023).
- **Compatibility and popularity**: The popularity and extensive usage of Keycloak become evident by looking at the large number of application stacks utilizing it compared to other SSO services. In addition, its compatibility with Spring Security is highly regarded by senior developers, demonstrating its smooth integration with Spring (Boot) in the comment section of article (_Firebase Authentication Vs Keycloak | What Are the Differences?_, n.d.). 
- **Supports social login**: Keycloak provides great support for social logins, enabling integration with social media authentication platforms. It also offers clear documentation to assist developers (Ekta, 2022).

Now Keycloak has met the requirements, the next step is to test its security capabilities. In the next section, I will conduct a security test and explore its other features to determine if Keycloak is the right SSO solution for addressing the identified vulnerabilities and protecting against common security threats described in the [Problem Analysis](#3-problem-analysis) section. 

___
[^5]: Keycloak is an open-source identity and access management solution that provides features such as user registration, authentication, authorization, and user profile management. Available at: https://www.keycloak.org/

## 6. Testing the Security of Keycloak

In this section, I will perform a credential stuffing attack using the Burp Suite tool to test the security measures provided by Keycloak within CineMatch, my own application, where I implemented it in the api-gateway service as a literal 'gatekeeper'. I will also explore additional security features that the service offers.

### 6.1 Credential Stuffing attack

Using the recommended Burp Suite tool in section [4. What can we do to prevent identification and authentication failures?](#4-what-can-we-do-to-prevent-identification-and-authentication-failures), I will simulate a credential stuffing attack against my application[^6]. This test aims to assess the resilience of Keycloak in detecting and mitigating such attacks. By using a small list of credentials, I will attempt to get unauthorized access to user accounts and show the effectiveness of Keycloak's protection mechanisms, including using one actual working combination of username and password. 

  <img src="/Media/credential_stuffing_burp_suite.png" alt="Simulation of a credential stuffing attack, through Burp Suite." width="800"> <br>
 *Simulation of a credential stuffing attack, through Burp Suite.*

In this screenshot, a couple of things are going on. At the bottom, there is an intercepted and modified HTTP request header sent to CineMatch by Burp Suite. Burp Suite contains a lot of functionalities, including request interception. The top section shows two payload lists being tested: one representing an email (used for login) and a password. The only combination that exists within Keycloak is `maurice@fontys.nl` with the password `fontys`. However, as indicated by the 302 "Found" response, which is a redirecting response, Keycloak handles the credential stuffing attack by redirecting the user to the login page. This behaviour is consistent for all the requests, suggesting that Keycloak has a built-in mechanism to handle brute force attacks in this manner. The HTTP response gathered from this attack contains a lot of interesting elements on how Keycloak implemented security measures, as we can see from this screenshot:

  <img src="/Media/http_response_burp_suite.png" alt="HTTP response to the correct login attempt, following a credential stuffing attack." width="800"> <br>
  *HTTP response to the correct login attempt, following a credential stuffing attack.* 

A summary of what we're seeing and why they help:

1. Referrer-Policy: no-referrer
    - This header controls how much information is included in the `Referer` header when making requests from one page to another. By setting it to "no-referrer," the browser will not send the `Referer` header, which helps prevent leakage of sensitive information.
2. X-Frame-Options: SAMEORIGIN
    - The X-Frame-Options header helps prevent clickjacking attacks by specifying whether a web page can be embedded within a frame or iframe. Setting it to "SAMEORIGIN" restricts framing to the same origin, preventing attackers from embedding the targeted page on their malicious sites.
3. Strict-Transport-Security: max-age=31536000; includeSubDomains
    - This header enables HTTP Strict Transport Security (HSTS) and instructs the browser to only communicate with the server over HTTPS. The `max-age` value specifies the duration in seconds that the browser should enforce HSTS. The `includeSubDomains` directive extends HSTS protection to all subdomains.
4. X-Robots-Tag: none
    - The X-Robots-Tag header controls how search engines and other bots index and cache the content of a web page. Setting it to "none" instructs bots not to index or cache the page, providing an additional layer of protection against unauthorized access.
5. Cache-Control: no-store, must-revalidate, max-age=0
    - This header controls caching behaviour. By specifying "no-store" and "must-revalidate," it prevents the browser and intermediary caches from storing a cached copy of the response. The `max-age` value of 0 ensures that the response is always considered stale and requires revalidation with the server.
1. X-Content-Type-Options: nosniff
    - This header prevents the browser from interpreting the response's Content-Type header in an unintended way. Setting it to "nosniff" ensures that the browser adheres strictly to the declared Content-Type and mitigates potential MIME type sniffing attacks.
7. Content-Security-Policy: frame-src 'self'; frame-ancestors 'self'; object-src 'none';
    - The Content-Security-Policy (CSP) header defines a policy that specifies which resources can be loaded and executed on a web page. In this case, the policy restricts framing to the same origin and disallows the loading of any external objects. Working in combination with X-FRAME-OPTIONS.
8. X-XSS-Protection: 1; mode=block
    - This header enables the browser's built-in Cross-Site Scripting (XSS) protection mechanism. Setting it to "1" and "mode=block" instructs the browser to block any detected XSS attacks and prevent the rendering of the attacked page.

By including these headers in the response, Keycloak shows how it deals with credential stuffing attacks. Each header handles a different, specific attack and provides defences at various stages of the request-response cycle. It is also possible to change the configuration of how it should deal with such requests within Keycloak:

  <img src="/Media/configuring_security_defenses_keycloak.png" alt="Header configuration within Keycloak." width="800"> <br>
  *Header configuration within Keycloak.*

When a real user enters a username and password that do not match or do not exist, Keycloak's response makes sure that sensitive information is not exposed. The screenshot below shows the response a user receives in such cases:

<img src="/Media/bad_credentials_keycloak.png" alt="Screenshot of Keycloak's response to bad credentials without exposing sensitive information." width="800"> <br> *Screenshot of Keycloak's response to bad credentials without exposing sensitive information.*

By providing a generic error message without specifying the exact reason for the authentication failure, Keycloak helps prevent potential attackers from gathering information about valid usernames or existing accounts. This approach covers one of the mentioned strategies in the previous section, [4. What can we do to prevent identification and authentication failures?](#4-what-can-we-do-to-prevent-identification-and-authentication-failures), which discusses the importance of not sharing any sensitive information in error messages. 


### 6.2 Exploring Keycloak's Additional Security Features

In order to tackle other common identification and authentication failures, it is possible to configure additional security features provided by Keycloak to further protect your application's security:

- CAPTCHA Integration: Keycloak supports the integration of CAPTCHA during the registration process[^7]. Specifically, it supports the integration of "ReCAPTCHA": a CAPTCHA system developed by Google. Using this helps prevent abuse by bots and unauthorized account creation.

- Enforcing Strict Password Policies: Weak passwords can be mitigated through setting up password policies. Keycloak offers various options that can be easily added to the registration system with a simple click.
  <img src="/Media/configure_password_policies_keycloak.png" alt="Options for password policies in Keycloak." width="800"> <br>
  *Options for password policies in Keycloak.*
- Prevention of Brute-Force Attacks: Keycloak has a way to mitigate brute-force attacks by limiting the number of login attempts within a specific time frame. By enabling and customizing the brute force detection in the Realm Settings, you can protect user accounts from unauthorized access through repeated login attempts. Please note that this feature was not enabled during the credential stuffing attack performed earlier.

  <img src="/Media/enable_and_configure_brute_force_prevention_keycloak.png" alt="Configuration for setting up brute force with a lot of different possibilities." width="800">
  <br>
*Configuration for setting up brute force with a lot of different possibilities.*
  
- Secure Session Management: Keycloak has a lot of options for session management. You can easily monitor active users, their associated clients, login timestamps, and IP addresses. You can also log out users from their sessions:

  <img src="/Media/session_management_keycloak.png" alt="Session management in Keycloak." width="800"> <br>
*Session management in Keycloak.*

---
[^6]: Learn more about credential stuffing attacks and Burp Suite here: [Credential stuffing with Burp Suite](https://portswigger.net/burp/documentation/desktop/testing-workflow/authentication-mechanisms/credential-stuffing)
[^7]: Find more information on enabling CAPTCHA integration in Keycloak here: [Enabling reCAPTCHA](https://www.keycloak.org/docs/latest/server_admin/#proc-enabling-recaptcha_server_administration_guide)

## 7. Conclusion

Putting all the different information gathered in the previous sections together, we can formulate a possible answer to the main question: **How can we effectively find and prevent identification and authentication failures in a distributed web application?**

The OWASP organization provides valuable resources that can serve as a checklist for finding potential authentication vulnerabilities in your application. Through my research, I have discovered that using a SSO service is an effective approaches to address these vulnerabilities and safeguard your application. By conducting an analysis of available SSO services, I decided to use Keycloak for my application. After some testing and examining additional security features of Keycloak, it is safe to conclude it is capable of handling common threats and enhance application security. By delegating the responsibilities of authentication and authorization to Keycloak, my application's security significantly improved, allowing me as a developer to focus on other development aspects.

While Keycloak has shown that it keeps up with best practices and its ability to mitigate common threats, it is important to remember that it is only one among several SSO options you can choose from. Other SSO solutions may also meet the requirements and pass similar tests, while some may not. In fact, there may even be alternative approaches altogether to tackle this problem. It is important for developers to explore and evaluate various options to determine the most suitable solution for their project.

In conclusion, by making use of the resources provided by organizations like OWASP and making research-based decisions when choosing and implementing a SSO service, developers can boost the security of their distributed web applications and create a strong and reliable authentication process.

## 8. References

_A07 Identification and Authentication Failures - OWASP Top 10:2021_. (n.d.). https://owasp.org/Top10/A07_2021-Identification_and_Authentication_Failures/

Ekta. (2022). The MSSP Guide to Keycloak. _Identity Management Solution & MSSP Company_. https://sennovate.com/the-mssp-guide-to-keycloak/

_Firebase Authentication vs Keycloak | What are the differences?_ (n.d.). StackShare. https://stackshare.io/stackups/firebase-authentication-vs-keycloak

Frontegg. (2021). Top 10 SSO Providers You Must Consider in 2022. _Frontegg_. https://frontegg.com/blog/top-sso-providers

_Identification & Authentication: Similarities & Differences | Okta_. (n.d.). Okta, Inc. https://www.okta.com/identity-101/identification-vs-authentication/

Kapoor, A. (2022, January 5). How to Secure Web Apps — A Web App Security Checklist. _Medium_. https://medium.com/quick-code/how-to-secure-web-apps-a-web-app-security-checklist-bb27cf049d1d

Ng, A. (2018, August 9). Why more people don’t use simple two-factor authentication. _CNET_. https://www.cnet.com/news/privacy/why-more-people-dont-use-simple-two-factor-authentication/

NOS. (2021, October 5). Hack bij HAN-hogeschool trof mogelijk ruim half miljoen mensen. _NOS_. https://nos.nl/artikel/2400478-hack-bij-han-hogeschool-trof-mogelijk-ruim-half-miljoen-mensen

Świątek, B. (2023, March 31). _Keycloak SSO – advantages of Single Sign-On and a ready-made access management system. Pretius. https://pretius.com/blog/keycloak-sso/

The Advantages and Benefits of Single Sign-On (SSO). (2023, January 3). _Okta, Inc._ https://www.okta.com/uk/blog/2022/04/benefits-of-single-sign-on/