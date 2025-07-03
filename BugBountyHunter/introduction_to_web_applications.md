# Introduction to Web Applications

Web applications are interactive applications that run on web browsers. Web
applications usually adopt a client-server architecture to run and handle
interactions. They typically have front end components (i.e., the website
interface, or "what the user sees") that run on the client-side (browser) and
other back end components (web application source code) that run on the
server-side (back end server/databases).

## Websites vs. Web Applications

### Websites:
- static pages
- same for everyone
- Web 1.0

### Web Applications
- dynamic pages
- interactive
- different view for each user
- functional
- Web 2.0

## Security Risks of Web Applications

To properly pentest web applications, we need to understand how they work, how
they are developed, and what kind of risk lies at each layer and component of
the application depending on the technologies in use.


We will always come across various web applications that are designed and
configured differently. One of the most current and widely used methods for
testing web applications is the OWASP Web Security Testing Guide.


One of the most common procedures is to start by reviewing a web application's
front end components, such as HTML, CSS and JavaScript (also known as the front
end trinity), and attempt to find vulnerabilities such as Sensitive Data
Exposure and Cross-Site Scripting (XSS). Once all front end components are
thoroughly tested, we would typically review the web application's core
functionality and the interaction between the browser and the webserver to
enumerate the technologies the webserver uses and look for exploitable flaws. We
typically assess web applications from both an unauthenticated and authenticated
perspective (if the application has login functionality) to maximize coverage
and review every possible attack scenario.


Web applications provide a vast attack surface, and their dynamic nature means
that they are constantly changing (and overlooked!). A relatively simple code
change may introduce a catastrophic vulnerability or a series of vulnerabilities
that can be chained together to gain access to sensitive data or remote code
execution on the webserver or other hosts in the environment, such as database
servers.

A well-rounded infosec professional should have a deep understanding of web
applications and be as comfortable attacking web applications as performing
network penetration testing and Active Directory attacks. A penetration tester
with a strong foundation in web applications can often set themselves apart from
their peers and find flaws that others may overlook


| Flaw                                       | Real-world Scenario |
|--------------------------------------------|---------------------|
| SQL injection                              | Obtaining Active Directory usernames and performing a password spraying attack against a VPN or email portal. |
| File Inclusion                             | Reading source code to find a hidden page or directory which exposes additional functionality that can be used to gain remote code execution. |
| Unrestricted File Upload  | A web application that allows a user to upload a profile picture that allows any file type to be uploaded (not just images). This can be leveraged to gain full control of the web application server by uploading malicious code. |
| Insecure Direct Object Referencing (IDOR)  | When combined with a flaw such as broken access control, this can often be used to access another user's files or functionality. An example would be editing your user profile browsing to a page such as /user/701/edit-profile. If we can change the 701 to 702, we may edit another user's profile! |
| Broken Access Control                      | Another example is an application that allows a user to register a new account. If the account registration functionality is designed poorly, a user may perform privilege escalation when registering. Consider the POST request when registering a new user, which submits the data username=bjones&password=Welcome1&email=bjones@inlanefreight.local&roleid=3. What if we can manipulate the roleid parameter and change it to 0 or 1. We have seen real-world applications where this was the case, and it was possible to quickly register an admin user and access many unintended features of the web application. |
