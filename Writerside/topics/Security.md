# Security

After I finished my research into security, I mentioned some actions that I can take to improve the overall security of my application. This document goes over all the steps I took to secure my application.

## Task List

| Task  	                                                                                                                                                                               | Going to do  	 | Notes  	                                            |
|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------|-----------------------------------------------------|
| Protected endpoints in the API-Gateway	                                                                                                                                               | 	V             | 	                                                   |
| Using Dependabot to keep dependencies up-to-date	                                                                                                                                     | 	V             | 	                                                   |
| Using the ZAP security scanner to scan the application for vulnerabilities	                                                                                                           | 	 V            | 	                                                   |
| Where possible, implement multi-factor authentication to prevent automated credential stuffing, brute force, and stolen credential reuse attacks.                                     | 	 X            | 	                                                   |
| Implement proper logging in order to detect breaches	                                                                                                                                 | 	 V            | A task that happens continuously	                   |
| Implement weak password checks, such as testing new or changed passwords against the top 10,000 worst passwords list.	                                                                | 	 V            | Auth0 handles large part of the auth process	       |
| Use the OWASP Dependency Check to scan the application for known vulnerabilities in the dependencies used	                                                                            | 	 X            | 	                                                   |
| Set up mandatory code reviews to make sure no malicious code is being added to the application	                                                                                       | 	 X            | In a team this would be necessary	                  |
| Set up a CORS policy to prevent SSRF attacks	                                                                                                                                         | 	   X          | 	                                                   |
| Ensure that logs are generated in a format that log management solutions can easily consume.	                                                                                         | 	V             | Monitoring tools can access the container/pod logs	 |
| Using the ZAP security scanner to scan the application for vulnerabilities	                                                                                                           | V	             | 	                                                   |
| Write unit and integration tests to validate that all critical flows are resistant to the threat model. Compile use-cases and misuse-cases for each tier of your application.	        | 	V             | 	                                                   |
| Limit resource consumption by user or service	                                                                                                                                        | 	V             | 	                                                   |


## Protected Routes

One of the first things the OWASP Top 10 mentions is `broken access control`. This is when a user can access a resource that they shouldn't be able to access. To prevent this, I implemented protected routes. In the context of my project this would mean that a user can only access certain routes, if they are logged in. If they aren't logged in, the backend will return a 401 status code.

In order to do this, I use the authentication service `Auth0`. This service provides me with a token that I can use to authenticate the user. When a user tries to access a protected route, the frontend will send this token to the backend. The backend will then verify this token with Auth0. If the token is valid, the user is allowed to access the route. If the token is invalid, the backend will return a 401 status code.

To implement this I had to add the following:

```JSON
"AuthenticationOptions": {
    "AuthenticationProviderKey": "Bearer",
    "AllowedScopes": []
}
```

When a route has this property present, it will expect a token to be present in the header of the request. If the token is not present, the backend will return a 401 status code.

To test this in Postman, I send a request to a protected route without a token. In this case it's the `POST /api/comments` route. This is the response I get:

![postman-401-response.png](postman-401-response.png)

As the screenshot shows, the backend denies access to this endpoint, because of the missing token.

When I do add a valid token to the request, the backend will allow access to the route. This is the response I get:

![postman-200-response.png](postman-200-response.png)

When adding a valid token, the backend will allow the request to go through and perform the action.

Another type of test I did was to send a request to a protected route with a manually modified token. Since the token is being validated with Auth0, it will only accept tokens that are generated by Auth0. This is the response I get:

![postman-401-invalid-token.png](postman-401-invalid-token.png)

Since the validation doesn't pass, the backend treats it as if there is no token present at all and returns a 401 status code.

This is done by setting up the middleware and specifying the `Audience` and `Authority`:

```C#
builder.Services.AddAuthentication(sharedOptions =>
{
    sharedOptions.DefaultAuthenticateScheme = JwtBearerDefaults.AuthenticationScheme;
    sharedOptions.DefaultChallengeScheme = JwtBearerDefaults.AuthenticationScheme;
})
.AddJwtBearer(options =>
{
    options.Authority = $"https://{builder.Configuration["Auth0:Domain"]}/";
    options.Audience = builder.Configuration["Auth0:Audience"];
});
```

## Rate Limiting

Another thing the OWASP Top 10 mentions is `insecure design`. They make a point within this category to `limit resource consumption by user or service`. This is a more general but important point, because you don't want users to have unlimited access to your resources. To prevent this, I used Ocelot's rate limiting middleware.

This allows me to set a limit on how many requests a user can make in a certain time frame. If a user exceeds this limit, the backend will return a custom status code and the user will need to wait a certain amount of time before they can make another request.

To implement this I had to add the following:

```JSON
"RateLimitOptions": {
    "ClientWhitelist": [
      "https://artillery.io"
    ],
    "EnableRateLimiting": true,
    "Period": "1s",
    "PeriodTimespan": 60,
    "Limit": 100
},
```

It shows that you can also whitelist certain clients, so they can make more requests than the limit. In my case I needed it for my load testing tool `Artillery`. This setup dictates that a user can make 100 requests per second. If they exceed this limit, the backend will return a `429` status code and they will have to wait 60 seconds before they can make another request.

In addition to the above object, I also had to add an object in the highest level of this file, which will contain more general rules. This object looks like this:

```JSON
"GlobalConfiguration": {
    "RateLimitOptions": {
        "DisableRateLimitHeaders": false,
        "QuotaExceededMessage": "Saddle down buckaroo! You've hit the rate limit! Please try again later.",
        "HttpStatusCode": 429,
        "ClientIdHeader": "ClientId"
    }
}
```

After this is all set up, I can test this in Postman. I send many requests to a route that has rate limiting enabled to see what kind of response it will give the client. In this case I tested the `GET /api/posts` route. This is the response I get:

![postman-429-response.png](postman-429-response.png)

As the screenshot shows, the backend returns a `429` status code and the custom message I set up in the global configuration object.

## Final Route

After having implemented these changes, the routes change a bit in structure. Because these gateway's are written using a JSON file, eventually you get one large list of objects, which will only continue to grow. The final route will look like this:

```JSON
{
  "UpstreamPathTemplate": "/api/comments/{everything}",
  "UpstreamHttpMethod": [ "Post", "Put", "Delete" ],
  "DownstreamHostAndPorts": [ { "Host": "comment-service", "Port": 8082 } ],
  "DownstreamPathTemplate": "/api/comments/{everything}",
  "RateLimitOptions": {
    "ClientWhitelist": [
      "https://artillery.io"
    ],
    "EnableRateLimiting": true,
    "Period": "1s",
    "PeriodTimespan": 60,
    "Limit": 100
  },
  "AuthenticationOptions": {
    "AuthenticationProviderKey": "Bearer",
    "AllowedScopes": []
  }
}
```

One of the downsides of these files is that you can get a lot of duplicate code, which can make reading a file like this a bit difficult.

## Dependabot

Another action that came out of the research was to use Dependabot. This is a tool that will automatically create pull requests when a new version of a dependency is released. This is important because you want to keep your dependencies up to date, this is part of the vulnerability in the OWASP Top 10 called `vulnerable and outdated components`.

After doing some research, I found out that Dependabot is a tool that is integrated into GitHub. This means that I don't have to install anything, I just have to enable it in the settings of my repository. After enabling it, it will automatically create pull requests when a new version of a dependency is released.

![repository-security-tab.png](repository-security-tab.png)

Although the setup of the tool is very minimal, it handles a very important aspect of a project. It will keep your dependencies up to date, which is important for the overall security of your application.

After enabling Dependabot, once in a while I will get an alert regarding a new version of a dependency. Below screenshot displays the alerts list from another project which has Dependabot enabled.

![dependabot-alerts-example.png](dependabot-alerts-example.png)

When I click on one of these alerts, it will show me details about the dependency and the new version.

![dependabot-alerts-details-example.png](dependabot-alerts-details-example.png)

## Zap Scan

Another action that came out of the research was to use OWASP ZAP scan. This is a tool that will scan your application for vulnerabilities. It's very easy to integrate into your CI/CD pipeline, with a template ready on the Github Actions marketplace.

![github-actions-zap-scan.png](github-actions-zap-scan.png)

After it is done scanning, it will create an issue with the issues it found. This is an example of what the issue looks like:

![zap-scan-issue.png](zap-scan-issue.png)

This issue shows a list of issues, for example missing `Content Security Policy` headers or `Anti-clickjacking` headers. Along with the static code analysis, this tool will help you find vulnerabilities in your application.