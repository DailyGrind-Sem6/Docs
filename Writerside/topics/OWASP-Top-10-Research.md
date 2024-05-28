# OWASP Top 10 Research

This document contains my research on OWASP Top 10 and what I need to do for my application to comply with it.

## How can I ensure my application meets OWASP requirements?

Although this is a broad question, I'm simply going to go through each security risk one-by-one and see if my application is vulnerable to it. If it is, I will also look into the necessary steps to fix it.

## What is OWASP Top 10?

According to the official OWASP website [[1]](#references), the OWASP Top 10 is:

> The OWASP Top 10 is a standard awareness document for developers and web application security. It represents a broad consensus about the most critical security risks to web applications.

Meaning it is a list of the top 10 security risks that web applications face. Developers can use this list to go through the potential risks and make sure their application isn't vulnerable to them.

## Top 10 Web Application Security Risks

The same source [[1]](#references) also lists the top 10 security risks with a link to a seperate page which goes into detail about each risk. I will go through each of these risks and see if my application is vulnerable to them.

### Broken Access Control

The dedicated page for this risk mentions:

> Access control enforces policy such that users cannot act outside of their intended permissions. Failures typically lead to unauthorized information disclosure, modification, or destruction of all data or performing a business function outside the user's limits.

This tells us that the risk is about users being able to access data or perform actions that they shouldn't be able to.

Since my application uses Auth0 for authentication, this can be mitigated by setting up the proper checks for each endpoint for instance. For most endpoints, a user should be authenticated in order to be able to make use of the endpoint.

My API-Gateway, created using Ocelot, supports authentication and authorization [[2]](#references). This means that I can set up the necessary checks for each endpoint in the API-Gateway.

Ocelot allows me to specify whether a user needs to be loggedin in order to access an endpoint, like this:

```json
{
    "UpstreamPathTemplate": "/posts",
    "UpstreamHttpMethod": [ "GET" ],
    "AuthenticationOptions": {
        "AuthenticationProviderKey": "Bearer",
        "AllowedScopes": []
    }
}
```

This example shows that the user needs to be logged in in order to access the `/posts` endpoint, because I specify the `Bearer` authentication provider key.

Another easy-to-fix issue, mentioned by the same page, is to correctly set up the CORS policy. This should be done in order to prevent access to untrusted origins. This would mean that only my frontend application can access the backend.

### Cryptographic Failures

The dedicated page for this risk mentions:

> The first thing is to determine the protection needs of data in transit and at rest. For example, passwords, credit card numbers, health records, personal information, and business secrets require extra protection, mainly if that data falls under privacy laws, e.g., EU's General Data Protection Regulation (GDPR), or regulations, e.g., financial data protection such as PCI Data Security Standard (PCI DSS).

This talks about the importance of encrypting sensitive data. My application doesn't store/handle much sensitive data, the only data that could be considered sensitive is the user's email and password. These are already being handled by Auth0, therefore I'm not storing them in my own database. 

Other kinds of data used in my application are articles and comments. These are not sensitive data and therefore don't need to be encrypted.

Although like mentioned under the *Cryptographic Failures* chapter, it is important to secure your application's communication with the server. This can be done by using HTTPS and TLS.

### Injection

The dedicated page for this risk mentions:
> An application is vulnerable to attack when:
>
> - User-supplied data is not validated, filtered, or sanitized by the application.
>
> - Dynamic queries or non-parameterized calls without context-aware escaping are used directly in the interpreter.
>
> - Hostile data is used within object-relational mapping (ORM) search parameters to extract additional, sensitive records.
>
> - Hostile data is directly used or concatenated. The SQL or command contains the structure and malicious data in dynamic queries, commands, or stored procedures.

This risk is about attackers being able to inject malicious code into the application. Meaning you shouldn't trust user input and always validate it.

In my application, I use the C# driver for MongoDB. According to the official MongoDB documentation [[3]](#references), MongoDB addresses SQL or query injection by using BSON objects to represent queries. The page gives an example:

> Here, `my_query` then will have a value such as `{ name : "Joe" }`. If `my_query` contained special characters, for example `,`, `:`, and `{`, the query simply wouldn't match any documents. For example, users cannot hijack a query and convert it to a delete.

This means that my application is not vulnerable to injection attacks, as long as I don't use a select amount of expressions. The page continues by saying:

> The following MongoDB operations permit you to run arbitrary JavaScript expressions directly on the server:
>
> - $where
>
> - mapReduce
>
> - $accumulator
> 
> - $function
>
> You must exercise care in these cases to prevent users from submitting malicious JavaScript.
>
> Fortunately, you can express most operations in MongoDB without JavaScript.

This shows that, were I to use the `$where` expression for example, my application would become vulnerable to injection attacks. Therefore, I avoid using these expressions in my application by making use of filters, like so:

```csharp
    var filter = Builders<Post>.Filter.Eq(post => post.Id, id);
    var result = _posts.Find(filter).SingleOrDefaultAsync();
```

### Insecure Design

The dedicated page for this risk mentions:

> Insecure design is a broad category representing different weaknesses, expressed as “missing or ineffective control design.” Insecure design is not the source for all other Top 10 risk categories. There is a difference between insecure design and insecure implementation. We differentiate between design flaws and implementation defects for a reason, they have different root causes and remediation. A secure design can still have implementation defects leading to vulnerabilities that may be exploited. An insecure design cannot be fixed by a perfect implementation as by definition, needed security controls were never created to defend against specific attacks. One of the factors that contribute to insecure design is the lack of business risk profiling inherent in the software or system being developed, and thus the failure to determine what level of security design is required.

This risk is about the overall design of the application, including architecture and the way the application is built. This encompasses a lot of different things, but there are a few things that can be done.

One of the prevention methods mentioned on the page is to setup tests for the application, this can be done by setting up unit tests and integration tests. This can help to find vulnerabilities in the application before they are exploited.

Another point mentioned by the page reads as follows:

> Limit resource consumption by user or service

This can be done by setting up rate limiting for example. This can prevent attackers from making too many requests to the server in a short amount of time. I use the `Ocelot` package for my API-Gateway, which supports rate limiting [[4]](#references).

To make sure there are no other vulnerabilities, I have some measures I already have implemented/plan on implementing:

- I already use code analysis with SonarCloud to find security vulnerabilities in the code.
- I'm planning on implementing Dependabot, which can automatically update dependencies used in the application [[5]](#references). This can help to prevent vulnerabilities caused by outdated dependencies.
- I'm planning on using the ZAP security scanner to scan my entire application for security vulnerabilities [[6]](#references).

### Security Misconfiguration

The dedicated page for this risk mentions:

> The application might be vulnerable if the application is:
>
> - Missing appropriate security hardening across any part of the application stack or improperly configured permissions on cloud services.
>
> - Unnecessary features are enabled or installed (e.g., unnecessary ports, services, pages, accounts, or privileges).
> 
> - Default accounts and their passwords are still enabled and unchanged.
> 
> - Error handling reveals stack traces or other overly informative error messages to users.

The page mentions a few things that can cause security misconfigurations, like not keeping unused entities enabled, like default accounts or unused ports. This can be prevented by making sure to disable these entities.

My application has no default accounts, I also don't have any unused services or ports enabled. My error handling is set up to not reveal any sensitive information, but instead return a generic error message. I detect when something goes wrong and return a generic error message to the user:

```csharp
if (post == null)
{
    throw new Exception("Post not found");
}
```

Along with this generic error message, I can also log the error in the backend, so a record is kept of everything that goes wrong.

### Vulnerable and Outdated Components

The dedicated page for this risk mentions:

> You are likely vulnerable:
>
> - If you do not know the versions of all components you use (both client-side and server-side). This includes components you directly use as well as nested dependencies.
>
> - If the software is vulnerable, unsupported, or out of date. This includes the OS, web/application server, database management system (DBMS), applications, APIs and all components, runtime environments, and libraries.
> 
> - If you do not scan for vulnerabilities regularly and subscribe to security bulletins related to the components you use.
> 
> - If you do not fix or upgrade the underlying platform, frameworks, and dependencies in a risk-based, timely fashion. This commonly happens in environments when patching is a monthly or quarterly task under change control, leaving organizations open to days or months of unnecessary exposure to fixed vulnerabilities.
> 
> - If software developers do not test the compatibility of updated, upgraded, or patched libraries.

This entire section talks about the importance of keeping all of your dependencies up-to-date, and not working with dependencies that aren't regularly supported for example.

One of the ways I'm planning on fixing this is by using Dependabot, which is a system that will automatically scan my repositories and update dependencies when a new version is available. This should help me a lot when it comes to keeping my dependencies up-to-date.

### Identification and Authentication Failures

The dedicated page for this risk mentions:

> Confirmation of the user's identity, authentication, and session management is critical to protect against authentication-related attacks. There may be authentication weaknesses if the application:
>
> - Permits automated attacks such as credential stuffing, where the attacker has a list of valid usernames and passwords.
>
> - Permits brute force or other automated attacks.
>
> - Permits default, weak, or well-known passwords, such as "Password1" or "admin/admin".
>
> - Uses weak or ineffective credential recovery and forgot-password processes, such as "knowledge-based answers," which cannot be made safe.
>
> - Uses plain text, encrypted, or weakly hashed passwords data stores (see A02:2021-Cryptographic Failures).

This talks about multiple areas of authentication, one of them being the process of logging in or recovering a password. My authentication is handled by Auth0, which is a well-known authentication provider. This means that such security risks are already mitigated by Auth0.

Along with this, I can also set up rate limiting for the login endpoint, to prevent attackers from making too many requests in a short amount of time. This can be done using the `Ocelot` package, which supports rate limiting [[4]](#references).

Any admin account, or any account with elevated permissions, will be given strong passwords.

### Software and Data Integrity Failures

The dedicated page for this risk mentions:

> Software and data integrity failures relate to code and infrastructure that does not protect against integrity violations. An example of this is where an application relies upon plugins, libraries, or modules from untrusted sources, repositories, and content delivery networks (CDNs). An insecure CI/CD pipeline can introduce the potential for unauthorized access, malicious code, or system compromise. Lastly, many applications now include auto-update functionality, where updates are downloaded without sufficient integrity verification and applied to the previously trusted application.

Dependabot can also help with this, as it will also notify of any security vulnerabilities in the dependencies used in the application. Another security measure is to set up mandatory code reviews, to make sure that no malicious code or package is being added to the application.

Another measure given by the page is to use the OWASP Dependency Check:

> Ensure that a software supply chain security tool, such as OWASP Dependency Check or OWASP CycloneDX, is used to verify that components do not contain known vulnerabilities

This is a software composition analysis tool that can be used to scan the application for known vulnerabilities in the dependencies used [[8]](#references).

### Security Logging and Monitoring Failures
 
The dedicated page for this risk mentions:

> Returning to the OWASP Top 10 2021, this category is to help detect, escalate, and respond to active breaches. Without logging and monitoring, breaches cannot be detected. Insufficient logging, detection, monitoring, and active response occurs any time:
>
> - Auditable events, such as logins, failed logins, and high-value transactions, are not logged.
>
> - Warnings and errors generate no, inadequate, or unclear log messages.
>
> - Logs of applications and APIs are not monitored for suspicious activity.
>
> - Logs are only stored locally.
>
> - Appropriate alerting thresholds and response escalation processes are not in place or effective.

This page talks about the importance of proper logging and monitoring. This can be done by setting up a logging system that logs all important events in the application. The logging itself can be done on different levels like: `Debug`, `Info`, `Warning` and `Error`.

The logging is not only handled locally. My monitoring tool, New Relic, also displays the logs that happen inside my cluster. This way, I can monitor the logs from anywhere.

### Server-Side Request Forgery (SSRF)

The dedicated page for this risk mentions:

> SSRF flaws occur whenever a web application is fetching a remote resource without validating the user-supplied URL. It allows an attacker to coerce the application to send a crafted request to an unexpected destination, even when protected by a firewall, VPN, or another type of network access control list (ACL).

This risk is about an attacker editing, for instance, the URL of a request to make the application send a request to a different server. This is a vulnerability that can be prevented by not giving users the ability to temper with URLs. The input will be validated and then, if needed, it can be used to make a request.

Along with this, a proper CORS setup can prevent these attacks, by only allowing requests from trusted origins. This means that incoming requests can be assumed to be safe.

## References
- [1] OWASP Top Ten | OWASP Foundation. (z.d.). https://owasp.org/www-project-top-ten/
- [2] Authentication — Ocelot 23.2 documentation. (z.d.). https://ocelot.readthedocs.io/en/latest/features/authentication.html#id2
- [3] FAQ: MongoDB Fundamentals - MongoDB Manual V7.0. (z.d.). https://www.mongodb.com/docs/manual/faq/fundamentals/#how-does-mongodb-address-sql-or-query-injection-
- [4] Rate Limiting — Ocelot 23.2 documentation. (z.d.). https://ocelot.readthedocs.io/en/latest/features/ratelimiting.html
- [5] Dependabot. (z.d.). GitHub. https://github.com/dependabot
- [6] The ZAP homepage. (z.d.). ZAP. https://www.zaproxy.org/
- [7] About Dependabot security updates - GitHub Docs. (z.d.). GitHub Docs. https://docs.github.com/en/code-security/dependabot/dependabot-security-updates/about-dependabot-security-updates
- [8] OWASP Dependency-Check | OWASP Foundation. (z.d.). https://owasp.org/www-project-dependency-check/