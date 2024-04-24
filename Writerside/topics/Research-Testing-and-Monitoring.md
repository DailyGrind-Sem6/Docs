# Research Testing and Monitoring

## Which testing and monitoring strategies can be used to keep the reliability and performance optimal of a coffee-focused community platform?

This is a research document that will explore the different testing and monitoring strategies that can be used to keep the reliability and performance optimal of a coffee-focused community platform.

### What are the most effective testing methodologies to ensure the features of the platform stay in good working order?

Testing the features of your application is a crucial step in the entire software development process. After putting in a lot of effort into building a product, you want to make sure that product stays working. 

This can help you keep track of issues that might arise after working on another part of the product. Since an addition or change in one part of the application can affect another part, it's important to test the application. This is where testing methodologies come in.

#### Testing Methodologies

There are a number of testing methodologies that can be used to ensure the features of the platform stay in working order. Some of the most used methodologies include:

Unit Testing
: The process of testing individual components of a system in isolation.

Integration Testing
: The process of testing how individual components work together.

System Testing
: The process of testing the system as a whole.

Performance Testing
: The process of testing the performance of the system under different conditions.

These testing methodologies differ in various ways, one of them being the scope of the testing. Unit testing, for example, focuses on testing individual components of the system, while system testing focuses on testing the system as a whole. These differences cause the methodologies to be most useful in different situations and system-types.
This semester, focussing on enterprise-level software, I will need to use a combination of these methodologies to make sure my application is reliable and performs well.

#### When to use which methodology

These methodologies test different scopes of the system, making them more useful in some situations than others. When unit testing, one may test the individual pieces of logic in the system, giving you more precise control and feedback on the test.

Integration testing, on the other hand, tests multiple of these "units" together, testing the interaction between them aswell. This might give you less control since, if an error does occur, it might make it harder to find the source of the error.

#### Conclusion

Meaning, when the individual parts of the application aren't too complex with many steps, integration testing will suffice. Since there aren't many steps in the overall process, it will be easier to find the source of the error if one does arise.

When the application has grown more complex features and logic, it may be worth to write seperate tests for those individual sections. That way, you have more control over the tests and can pinpoint the source of the error more easily.

#### DOT Framework

**Best good and bad practices:**

I've explored various testing methodologies such as Unit Testing, Integration Testing, etc. I've evaluated their use and relevancy in my context and app structure. This involves understanding the scope of each testing methodology and when to use which methodology. 

**Available product analysis:**

I've looked into implementations of the test methods to prepare my own tests. This involves researching and referencing various resources such as articles, blogs, and tutorials on testing microservices, integration tests in ASP.NET Core, etc.

#### References

- [Testing microservices](https://www.zartis.com/testing-microservices/)
- [Writing tests for microservices](https://livebook.manning.com/book/microservices-in-net-core/chapter-7/)
- [Reddit - Integration test Vs System Test](https://www.reddit.com/r/embedded/comments/14lhaq8/integration_test_vs_system_test/)

*Testing implementation references:*

- [Microsoft - Integration tests in ASP.NET Core](https://learn.microsoft.com/en-us/aspnet/core/test/integration-tests?view=aspnetcore-8.0)
- [LinkedIn - Integration Test for Asp.Net Core API with Entity Framework using Testcontainers](https://www.linkedin.com/pulse/integration-test-aspnet-core-api-entity-framework-using-sarker/)
- [Youtube - ASP.NET Core Integration Testing Tutorial](https://www.youtube.com/watch?v=RXSPCIrrjHc)
