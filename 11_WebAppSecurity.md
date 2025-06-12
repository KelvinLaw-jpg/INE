# Web App Security

- website is a webpage that allows people to serve and doesn't have complex or much user interaction functionalities
- web app essentially are applications on web servers which users can interact with when browser to the server

## Web app security testing vs web app pentesting

- Web app pentesting is a sub set of web app security testing, below is what the web app security testing pipeline encompass, while web app pentesting focus on actively exploiting vulnerabilities
- First do a web app accessment and web app vuln scans first and then proceed to web app pentest, at last it is code review and static analysis (this can only be done by pentester that had experience with that language)
- Testing types consist: Authenticationa nd authorization testing, input validation and output encoding testing, session management testing, API security testing

### Common Web App Threats & Risks

- Threat is the danger such as malware, XXS, SQL injection etc, risk is the potential or the likelihood of the incident happening and the potential magnitude of its impact
- Most common Threats can be found at OWASP TOP 10, but here are some of them: XSS, SQLi, CSRF, Security Misconfigurations, Sensitive Data Exposure, BF and Credential Stuffing Attacks, File Upload Vulnerabilites, DoS/DDoS, SSRF, Broken or Inadequate AC, Using Components with Known Vuln, 


## Web App Architecture
Types: Client-Server model
- Client side: Anything processed on the user device, typically on the web browser
- Goal of web app attack, first question to ask: what can we change or do to affect the web server?
- Server-side: Anything processed on the web server, typically business logic, data processing, data storage

### Client-Side Technologies
- HTML: markup language used to structure and content of web pages (index.html)
- CSS: to define the presentation and styling of the web pages
- JavaScript: used for interactivity in web applications, used to create dynamic and responsive UI elements
- Cookies and Local Storage: to store small amounts of data on user's browser

### Server-Side Technologies
- Web Server: responsible for receiving and responding the HTTP requests from clients
- Application Server: here we are not saying the web server, we are refering to the web app itself which could reside on the web server. It process all the business logica and dynamic content
- Database Server: store user info and other web app data
- Languages: python, php, java, ruby

### Data Interchange (API)
- Data Interchange refers to the process of exchanging data between different computer systems or applications, aloowing them to communicate and share info
- enabling interoperability and data sharing between diverse systems, platforms and technologies
- It involves the conversion of data from one format to another, making it compatible with the receiving system
- Ensures data are interpreted and utilized correctly
- APIs are the core technology used to integrate with external services, share data, and provide functionalities to other app
- JSON: lightweight and widely used data interchange format based on JavaScript syntax and used to transmit data between server and a web app as alternative to XML
- XML: versatile data interchange format that uses tags to define the structure of the data
- REST (Representational State Transfer) API: Rest is a software architectural style that uses standard HTTP methods, it is widely used to creating web APIs
- SOAP (Simple Object Access Protocol): used for exchanging structured info in the implementation of seb services. It uses XML as the data interchange format and provides a standardized method for communication between different systems
- Authentication Mechanism: cookies, Authorization Mechanisms: user roles and permissions, Encryption: SSL/TLS
- External Technologies include Content Delivery Networks (CDNs), Third-Party Libraries and Frameworks
