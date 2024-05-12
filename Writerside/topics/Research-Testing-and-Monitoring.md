# Research Testing and Monitoring

## Which testing and monitoring strategies can be used to keep the reliability and performance optimal of a coffee-focused community platform?

This is a research document that will explore the different testing and monitoring strategies that can be used to keep the reliability and performance optimal of a coffee-focused community platform.

## What are the most effective testing methodologies to ensure the features of the platform stay in good working order?

Testing the features of your application is a crucial step in the entire software development process. After putting in a lot of effort into building a product, you want to make sure that product stays working. 

This can help you keep track of issues that might arise after working on another part of the product. Since an addition or change in one part of the application can affect another part, it's important to test the application. This is where testing methodologies come in.

Since there are different testing-methods, it's important to know which one to use in which situation, since different methods will be more useful in different situations. Because of this we come back to the question of **which testing methodologies are the most effective to ensure the features of the platform stay in good working order**.

### Testing Methodologies

There are a number of testing methodologies that can be used to ensure the features of the platform stay in working order. Some of the most used methodologies include:

Unit Testing
: The process of testing individual components of a system in isolation.

Integration Testing
: The process of testing how individual components work together.

System Testing
: The process of testing the system as a whole.

Performance Testing
: The process of testing the performance of the system under different conditions.

#### Unit testing
These testing methodologies differ in various ways, one of them being the scope of the testing. Unit testing, for example, focuses on testing individual components of the system in isolation. That means that you can test the individual pieces of logic in the system, giving you more precise control and feedback on the test.

![diagram-export-5-12-2024-6_12_39-PM.png](unit-test-area.png)

The area colored in red represents the scope of unit testing, which is in this case the service layer of the application. This layer handles the business logic of the application, meaning that such a test would test the individual pieces of logic in the system.

#### Integration testing

While integration testing focuses on testing multiple units together along with the interaction between them. This might give you less control since, if an error does occur, it might make it harder to find the source of the error.

![diagram-export-5-12-2024-6_18_27-PM.png](integration-test-area.png)

The area colored in red here represents the scope of integration testing, which is in this case the controller, service layer and the data access layer of the application. These layers handle the interaction between the different parts of the application, meaning that such a test would test the interaction between the different parts of the application.

These differences cause the methodologies to be most useful in different situations and system-types.
This semester, focussing on enterprise-level software, I will need to use a combination of these methodologies to make sure my application is reliable and performs well.

#### Load Testing

Load testing is another important testing methodology that can be used to ensure the features of the platform stay in good working order. Load testing is the process of testing the performance of the system under different load conditions. This can help you identify bottlenecks in the system and make sure it can handle the expected load.
This is done by simulating a large number of users accessing the system at the same time and measuring the performance of the system under these conditions. If the system can't handle the load, you might need to make some changes to the system to make sure it can handle the expected load.

### When to use which methodology

These methodologies test different scopes of the system, making them more useful in some situations than others. When unit testing, one may test the individual pieces of logic in the system, giving you more precise control and feedback on the test.

Integration testing, on the other hand, tests multiple of these "units" together, testing the interaction between them aswell. This might give you less control since, if an error does occur, it might make it harder to find the source of the error.

### Conclusion on Testing Methodologies

Meaning, when the individual parts of the application aren't too complex with many steps, integration testing will suffice. Since there aren't many steps in the overall process, it will be easier to find the source of the error if one does arise.

When the application has grown more complex features and logic, it may be worth to write seperate tests for those individual sections. That way, you have more control over the tests and can pinpoint the source of the error more easily.

### DOT Framework for Testing

**Best good and bad practices:**

I've explored various testing methodologies such as Unit Testing, Integration Testing, etc. I've evaluated their use and relevancy in my context and app structure. This involves understanding the scope of each testing methodology and when to use which methodology. 

**Available product analysis:**

I've looked into implementations of the test methods to prepare my own tests. This involves researching and referencing various resources such as articles, blogs, and tutorials on testing microservices, integration tests in ASP.NET Core, etc.

### References for Testing

- [Testing microservices](https://www.zartis.com/testing-microservices/)
- [Writing tests for microservices](https://livebook.manning.com/book/microservices-in-net-core/chapter-7/)
- [Reddit - Integration test Vs System Test](https://www.reddit.com/r/embedded/comments/14lhaq8/integration_test_vs_system_test/)

*Testing implementation references:*

- [Microsoft - Integration tests in ASP.NET Core](https://learn.microsoft.com/en-us/aspnet/core/test/integration-tests?view=aspnetcore-8.0)
- [LinkedIn - Integration Test for Asp.Net Core API with Entity Framework using Testcontainers](https://www.linkedin.com/pulse/integration-test-aspnet-core-api-entity-framework-using-sarker/)
- [Youtube - ASP.NET Core Integration Testing Tutorial](https://www.youtube.com/watch?v=RXSPCIrrjHc)

## How can automated code analysis frameworks streamline the testing process and enhance the efficiency of development iterations?

Code analysis tools are used to analyze the codebase of a software project. They check for different things such as code quality, security vulnerabilities, and performance issues. So just because your unit/integration tests have passed, doesn't mean your code doesn't contain any issues.

### Automated Code Analysis

Automated code analysis tools can help streamline the testing process and enhance the efficiency of development iterations. These tools can be used to check for various issues in the codebase, such as:

Code reliability
: The tool can check for issues that might cause the code to behave unexpectedly.

Code security
: The tool can check for security vulnerabilities in the codebase.

Code maintainability
: The tool can check for issues that might make the code hard to maintain.

### Added value

These tools bring the added value of preventing issues from happening in the first place. By analyzing the code and marking sections that might prove problematic, you can fix these issues before they cause a real problem in the application.

This in turn can save you time and money, since fixing issues after they've been deployed can be more costly than fixing them before they've been deployed. That is because you might have to do some debugging later on to find the source of the issue and then fix it, which is precious time that could've been spent on further development of the application.

### Analysis Tools

There are various code analysis tools that can be used to analyze the codebase of a software project. However the most used and known tools are:

- SonarQube (SonarCloud)
- Qodana (Jetbrains)

These tools mostly perform the same tasks, but they might have some differences in their pricing policies and installation processes.

### Conclusion on Automated Code Analysis

I decided to use [SonarCloud](https://www.sonarsource.com/products/sonarcloud/) for the automated code analysis of my application, since it has a generous free-tier which gives you the ability to test features out. It also has a cloud version which is very easy to set up and use. At my scope, it didn't matter much which one I chose, since they all perform the same tasks, but at a larger scale,a deeper analysis of the tools might be needed.

### DOT Framework for Automated Code Analysis

**Available product analysis**

I've looked into tools and platforms that may help me with automated code analysis. This involves researching and referencing various resources such as articles, blogs, and tutorials on SonarQube, Qodana, etc.

### References for Automated Code Analysis
- [OWASP - Static Code Analysis](https://owasp.org/www-community/controls/Static_Code_Analysis)
- [Static Code Analysis: Everything You Need to Know](https://blog.codacy.com/static-code-analysis)
- [Qodana](https://www.jetbrains.com/qodana/)

## What monitoring techniques should be implemented to detect potential issues in the platform?

Monitoring is the process of observing and checking various aspects of a system. Since a microservice consists of multiple smaller systems working together, it's important to make sure all running systems are working as expected.

### Metrics

Metrics are aspects of a system that can be measured. They can tell us something about the state of the system, how it's performing and any abnormalities that might be present.

Some metrics that can be measured in a microservice architecture include:

Response time
: The time it takes for a request to be processed, this can tell us something about the system's performance.

Error metrics
: The number of errors that occur in the system, this can be helpful in detecting issues.

Request volume
: The number of requests that are being made to the system, you can identify spikes in traffic and adjust resources as needed.

And there are more metrics that can be measured, depending on the system and what you want to know about it.

### Monitoring Tools

There are various tools that can be used to monitor a system. Some of the most used tools include:

- Prometheus
- Grafana
- Datadog

And ofcourse many more. These tools can be used to collect metrics from the system and display them in a way that's easy to understand. This can help you detect issues in the system and take action before they become a problem.

All of these tools provide monitoring features, although there is a difference in their pricing policies and installation processes.


|              | Grafana          | Prometheus | Datadog          |
|--------------|------------------|------------|------------------|
| Pricing      | Free tier + paid | /          | Free tier + paid |
| Installation | Cloud + local    | Local      | Local            |
{style="both"}

### Conclusion on Monitoring

I decided to use [Grafana](https://grafana.com/) for the monitoring of my application, since it has a generous free-tier which gives you the ability to test features out. It also, unlike the others, has a cloud version which is easier to set up and use.

Although, there is no right or wrong answer when it comes to monitoring tools, since they all perform more or less the same purpose. It's important to take a look at the project you work and see whether you have the need for a very specific feature and look for a tool that provides that.

### DOT Framework for Monitoring

**Best good and bad practices:**

I've explored various monitoring tools such as Prometheus, Grafana, etc. I've evaluated their use and relevancy in my context and app structure. This involves understanding the pricing policies and installation processes of each tool.

### References for Monitoring
- [Microservices Monitoring and Observability in Depth](https://medium.com/cloud-native-daily/microservices-monitoring-and-observability-in-depth-d40aa0795dd3)
- [HOW TO CHOOSE A MICROSERVICES MONITORING TOOL](https://redis.io/blog/choose-microservice-monitoring-tool/)