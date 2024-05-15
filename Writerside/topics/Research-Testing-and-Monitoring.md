# Research Testing and Monitoring

## Which testing and monitoring strategies can be used to keep the reliability and performance optimal of a coffee-focused community platform?

This is a research document that will explore the different testing and monitoring strategies that can be used to keep the reliability and performance optimal of a coffee-focused community platform.

## Which testing methods can be used to ensure the features of the coffee-based community platform are always working?

Testing the features of an application is a crucial step in the entire software development process. After putting in effort into building a product, it's also important to make sure that product stays working. 

Since there are different testing-methods, it's important to know which one to use in which situation, since different methods will be more useful in different situations. Because of this we come back to the question of **which testing methods can be used to ensure the features of the coffee-based community platform are always working**.

### Testing Methods

Let's take a look at the different testing methods that are available and see how they differ from each other.

#### Unit testing

Konghq.com describes unit tests as:
> At the bottom of the testing pyramid are unit tests, which focus on the smallest testable unit in your codebase, typically a method or a function. [[1]](#references-for-testing)

This means that unit tests only focus on the smallest piece of your code. This becomes useful when wanting to test isolated pieces of logic for example.

Konghq.com also mentions about unit tests:
> given a particular circumstance, when x happens, then the result should be y. [[1]](#references-for-testing)

They talk about the *given* element being anything that your unit may need in order to work properly. Dependencies on other collaborators are allowed, which would be called *sociable unit tests*. Or they may be replaced by "test doubles" in order to ensure isolation, called *solitary unit tests*. Which can be achieved by created mocks and stubs.

If this scope were to be visualized, it would look like this:

![unit-test-area.png](unit-test-area.png)

*A diagram highlighting in red the scope of unit testing in a system.*

If a unit test only tests the smallest piece of ones code, it would only encompass one layer of the application, in this case, the service layer.

#### Integration testing

As for integration tests, Konghq.com describes them as:
> Often microservices depend on calls to external modules, such as a data store or file system, and to other services. Integration tests verify that the interactions between these modules and services work as intended. [[1]](#references-for-testing)

This shows that the scope of the test is being widened, since it's not only testing the smallest piece of code, but also the interaction between different pieces of code. What the actual services are, can be different as mentioned in the quote, it can be a data store, file system or other services.

![integration-test-area.png](integration-test-area.png)

*A diagram highlighting in red the scope of integration testing in a system.*

If an integration test tests the interaction between different pieces of code, it would encompass multiple layers of the application, in this case, the service layer and the data access layer.

#### Component testing

Another test mentioned when it comes to testing microservices is component testing. Konghq.com describes component tests as:
> a component test verifies the behavior of an entire component in isolation. [[1]](#references-for-testing)
> 
> ...
> 
> How you define a component depends on your application, but a useful starting point is to think of each individual microservice as a component. [[1]](#references-for-testing)

This gives a clear definition of what a `component` is, since it could be interpreted in different ways. So in this case, a component test would test the behavior of an entire microservice in isolation.

This would add the entrypoint layer to the scope of the test, meaning sending a request to the controller methods and checking the response sent back.

![component-test-area](component-test-area.png)

*A diagram highlighting in red the scope of component testing in a system.*

This would be the final layer of tests that deal with one microservice in isolation.

#### Performance Testing

Another type of test mentioned online is Performance testing. According to browserstack.com, performance testing can be classified as:

> Performance Testing is a type of software testing for evaluating how a certain software performs under variant conditions. Performance, in this case, refers to multiple variables: stability, scalability, speed, and responsiveness – all under numerous levels of traffic and load. [[2]](#references-for-testing)

Out of this we can derive that performance testing simulates varying conditions to see how the software performs under different circumstances. When reading further performance testing is divided into different types of tests and explained what each test is used for [[2]](#references-for-testing) , such as:

- Load Testing
> Measures system performance under varying load levels, AKA the number of simultaneous users running transactions.

In the context of my project, this would be helpful. Since a community platform can have many users at the same time, it's important to know how the system performs under different load levels.

- Stress Testing
> Also known as fatigue testing, it measures how the system performs in abnormal user conditions. It is used to determine the limit at which the software actually breaks and malfunctions.

This method talks about abnormal user conditions, meaning a large volume of users. Community platforms aren't the type of applications that would see sudden influxes of large amounts of users visiting the platform for instance. This might be more useful for a platform like streaming site, where an event or a famous figure hosting a stream could draw in a large amount of users. 

- Endurance Testing
> Also known as soak testing, it evaluates how the software will perform under normal loads over a long period of time.

As mentioned by the description, this test uses a normal load over a long period of time. Like I said before, a community platform wouldn't necessarily see a large influx of users interacting with the platform, therefore this test wouldn't be as useful, to see what high traffic over a long period of time would do to the system.

- Spike Testing
> Repeatedly evaluates how the software performs when subjected to high traffic and usage levels in short periods.

Also for this method, the slower nature of a community platform wouldn't see sudden spikes in traffic, making this test less useful.

- Scalability Testing
> Determines if the software is handling increasing loads at a steady pace.

This test would simulate a more realistic scenario for a community platform, since the platform would grow over time and more users would interact with the platform. Unlike the other ones, which talk about a sudden spike or a large volume of users. With a test like this the scalability of the application can be tested and monitored to see whether the system correctly scales with the growth of the platform.

- Volume Testing
> Also known as flood testing because it involves following the system with data to check if it can be overwhelmed at any point and what fail-safes are necessary.

This test looks at the system being overwhelmed with data instead of users. Due to the nature of my version of a community platform, with larger texts being written in the form of articles/tutorials/guides, this test will not do the testing that is needed.

### Conclusion on Testing Methodologies

In conclusion, after examining various testing methods for microservices, I have been able to make a selection of some methods which seem to be most beneficial for my coffee-based community platform.

Unit testing will be used to ensure the smallest parts of the code function correctly.
Integration testing will verify that different components, such as data stores and services, interact properly. 
Component testing will check the behavior of entire microservices in isolation. 

For performance testing, load testing and scalability testing will be employed to evaluate how the platform performs under different user loads and as the community grows. 
I have selected these methods after considering the nature of the platform and the expected user interactions.

### DOT Framework for Testing

**Best good and bad practices:**

I've explored various testing methodologies such as Unit Testing, Integration Testing, etc. I've evaluated their use and relevancy in my context and app structure. This involves understanding the scope of each testing methodology and when to use which methodology. 

**Available product analysis:**

I've looked into implementations of the test methods to prepare my own tests. This involves researching and referencing various resources such as articles, blogs, and tutorials on testing microservices, integration tests in ASP.NET Core, etc.

### References for Testing

- [1] Kong. (z.d.). How to Test Microservices: Strategy and Use Cases. Kong Inc. https://konghq.com/blog/learning-center/microservices-testing-guide
- [2] Bose, S. (2022, 23 juli). What is Performance Testing : Detailed Guide | BrowserStack. BrowserStack. https://www.browserstack.com/guide/performance-testing

## How can automated code analysis frameworks streamline the testing process and enhance the efficiency of development iterations?

Code Analysis is a technique that is mentioned in various articles and blogs, but before I can determine how it can help my project, I need to understand what it is. According to this paper [[3]](#references-for-automated-code-analysis):

> Static code analysis is a technique used to analyze source code without executing it. It involves examining the code for potential defects, security vulnerabilities, coding style violations, and other issues using automated tools.

The paper goes on to give pros and cons of static code analysis:

| Benefits                                                                                                                                                | Limitations                                                                                                                                  |
|---------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| **Early defect detection:** Static code analysis identifies issues during the development phase, preventing them from propagating to downstream stages. | **False positives:** Static analysis tools may produce false positives, flagging code as problematic when it is not.                         |
| **Consistency:** It helps enforce coding standards and best practices across the codebase, improving maintainability and readability.                   | **Limited scope:** Static analysis cannot detect runtime errors or issues related to system interactions, which may require dynamic testing. |
| **Automation:** Static analysis tools can be integrated into CI/CD pipelines, automating the process of code review and defect detection.               | **Setup and configuration:** Configuring static analysis tools and interpreting their results may require expertise and effort.              |

Look at these benefits and limitations, the value that it can give you and your project, by detecting potential issues early in your code and notifying you, outweighs the limitations that come with it. When it flags issues, someone is going to have to take a look at it in order for it to be fixed, so if during that process it turns out to be a false positive, it's not a big deal.

and one of the limitations being `Static analysis cannot detect runtime errors or issues related to system interactions, which may require dynamic testing.`. This is where the other testing methods come in, such as integration testing. These methods can be used in conjunction with each other to make sure the code is working as expected. Therefore, the limitations of static code analysis are mitigated whe used in conjunction with other processes.

### Importance of Automated Code Analysis

One of the questions that comes up when looking into code analysis is why it's important/needed. According to Codium.ai [[4]](#references-for-automated-code-analysis), there's several reasons why code analysis is important:

- **Improve code quality:** Code analysis may assist in detecting coding flaws, performance concerns, and other issues that might lead to software failures, making it simpler to maintain and improve the code over time.
- **Satisfy compliance requirements:** Several businesses and government laws need certain security criteria to be met, and doing code analysis may assist in ensuring that the program fulfills those standards.
- **Improve productivity:** By detecting possible problems early in the development process, code analysis may assist in decreasing the time and effort required for testing and debugging, resulting in higher productivity and a shorter time-to-market.

So not only may it help with complying with governmental laws and standards, it may also help with improving the productivity of the development team. By detecting possible problems early in the development process, it may assist in decreasing the time and effort required for testing and debugging, resulting in higher productivity.

### Analysis Tools

When looking into code analysis tools, some are mentioned more frequently than others. Codium.ai mentions the following tools [[4]](#references-for-automated-code-analysis):

- SonarQube [[5]](#references-for-automated-code-analysis)
> an open-source tool for code quality and security analysis that supports a wide range of programming languages such as Java, C#, Python, JavaScript, and others.

| Pros   	                 | Cons   	                                        |
|--------------------------|-------------------------------------------------|
| Wide array of languages	 | Has to be integrated with on-premise solutions	 |
| Has a free tier to use	  | Has been reported to have not so ideal UI	      |

SonarQube like mentioned, has to be integrated with on-premise solutions, which might bring extra work with it. It also has a cloud version called SonarCloud, which is easier to set up and use.

- ESLint [[6]](#references-for-automated-code-analysis)
> is a free and open-source tool for detecting and reporting patterns in ECMAScript/JavaScript code.

| Pros   	                                                 | Cons   	                                                     |
|----------------------------------------------------------|--------------------------------------------------------------|
| Easy to install into projects                            | No support for other languages beside ECMAScript/JavaScript	 |
| Focusses on Javascript so it's specialized for that code | 	                                                            |
| It's free to use	                                        | 	                                                            |
 | Can be run in editor but also in CI/CD pipelines	        | 	                                                            |

Because it only focusses on Javascript, it's specialized for that code, since my frontend is built in React, this tool might be useful for that part of the codebase.

- ReSharper

ReSharper is a tool by Jetbrains, but it only focusses on .Net languages. Jetbrains also has a different tool called Qodana [[7]](#references-for-automated-code-analysis), which is a code analysis tool that supports multiple languages.

| Pros   	                | Cons   	                                |
|-------------------------|-----------------------------------------|
| Has a free tier         | Limited language support for free tier	 |
| IDE integration support | No frameworks support	                  |
| Quickfixes              | No security analysis	                   |

Qodana has some good features, although the free tier has some limitations that the other tools don't have. It also doesn't have security analysis, which is a big part of code analysis. it also has very limited langague support for the free tier.

### Conclusion on Automated Code Analysis

So I looked into why code analysis would be helpful for my project, and found that it may help with issues like improving code quality, satisfying compliance requirements and improving productivity. 

I also looked into some tools that can be used for code analysis, such as SonarQube, ESLint, and Qodana. I decided to use ESLint for the frontend part of my codebase, it being supported in editors makes it very handy to have since it'll give you feedback on your code as you write it. For the rest of my codebase, I'll be using SonarCloud, since it has a wide range of languages it supports, has a free tier to use and is cloud-based.

### DOT Framework for Automated Code Analysis

**Available product analysis**

I've looked into tools and platforms that may help me with automated code analysis. This involves researching and referencing various resources such as articles, blogs, and tutorials on SonarQube, Qodana, etc.

### References for Automated Code Analysis
- [3] Frank, L. F., & Mohamed, S. M. (2024, april). Enhancing Software Development Efficiency: CI/CD Pipelines with Real-Time Defect Detection. Researchgate. https://www.researchgate.net/publication/380288596_Enhancing_Software_Development_Efficiency_CICD_Pipelines_with_Real-Time_Defect_Detection
- [4] CodiumAI. (2023, 6 juli). What is Code Analysis. https://www.codium.ai/glossary/code-analysis/
- [5] Sonar. (z.d.-b). Code Quality Tool & Secure Analysis with SonarQube. https://www.sonarsource.com/products/sonarqube/
- [6] Find and fix problems in your JavaScript code - ESLint - Pluggable JavaScript Linter. (2024, 9 mei). https://eslint.org/
- [7] Qodana: Static Code Analysis Tool by JetBrains. (2021, 21 oktober). JetBrains. https://www.jetbrains.com/qodana/

## Which monitoring platform can be integrated to detect potential issues in the community platform?

Monitoring is described by kubernetes.io [[8]](#references-for-monitoring) as:

> To scale an application and provide a reliable service, you need to understand how the application behaves when it is deployed. You can examine application performance in a Kubernetes cluster by examining the containers, pods, services, and the characteristics of the overall cluster.

This can be used to keep an eye on your application and see how it performs, and it's health. This could help detect any anomalies in the system, because of which I can take action if needed.

### Metrics

When looking for which resource metrics are important and need monitoring, kubernetes.io [[9]](#references-for-monitoring) mentions the following:

- CPU
> CPU is reported as the average core usage measured in cpu units. One cpu, in Kubernetes, is equivalent to 1 vCPU/Core for cloud providers, and 1 hyper-thread on bare-metal Intel processors.

- Memory
> Memory is reported as the working set, measured in bytes, at the instant the metric was collected.

This gives me an idea of which metrics a monitoring tool should be able to measure and visualize. Next to these metrics, I would also like to measure application-specific metrics, like inbound traffic, response time, and error rates.

### Monitoring Tools

I found this clear article which gives some examples of monitoring tools that can be used for monitoring a kubernetes deployment [[10]](#references-for-monitoring). It's quite a bulky list, so I looked at the first bunch of tools. The ones that stood out to me were:

#### Helios

The Helios site [[11]](#references-for-monitoring) says Helios is a minotoring tool/observability platform designed for distributed systems. The site also shows that it supports kubernetes, specifically.

Here are some of the pros and cons of Helios:


| Pros                                                                 | Cons                                                                                |
|----------------------------------------------------------------------|-------------------------------------------------------------------------------------|
| E2E microservices visibility                                         | The docs page is inactive, so can't look into, for example the installation process |
| Collects all interactions, including HTTP and messaging payload data |                                                                                     |
| Easy installation                                                    |                                                                                     |
{style="both"}

Even though Helios seems like a great option, it's Docs page is inactive so I can't look into the installation process, or any other in depth information about the tool.

#### New Relic

The New Relic website ([newrelic.com](https://www.newrelic.com)) describes the tool as `All-in-one observability`. It says the tool will help developers `monitor, debug, and improve their entire stack`.

Let's look at some pros and cons of the tool:

| Pros                                           | Cons                                                                          |
|------------------------------------------------|-------------------------------------------------------------------------------|
| Lot's of integrations                          | Because of the different ways to install, the doc pages can be very cluttered |
| Tracks cluster resources (CPU/Memory) and more |                                                                               |
| Guided installation                            |                                                                               |

The kubernetes page of the New Relic website [[12]](#references-for-monitoring) gives a list of metrics it tracks, like:

- resources used
- number of K8s objects
- namespaces per cluster
- pods by namespace
- container cpu usage
- container restarts
- missing pods by deployment
- node resource consumption, and more

This makes New Relic a very good option for monitoring my kubernetes deployment. It monitors a lot of things and the installation seems easy to do.

#### Grafana

The Grafana website ([grafana.com](https://grafana.com)) describes the tool as `The open observability platform`, it also mentions Kubernetes observability.

Here are some pros and cons of Grafana:

| Pros                                         | Cons                                                 |
|----------------------------------------------|------------------------------------------------------|
| Generous free tier                           | It has a lot of features, can get lost in navigation |
| It has both a cloud and self-hosted solution |                                                      |
| Extensive documentation                      |                                                      |

### Conclusion on Monitoring

I looked through some monitoring tools and compared them to find a good fit for my project. They all seemed promising, but I decided to first try out New Relic. It tracks lots of integrations and the installation seems easy to do. If New Relic doesn't work out, I have chosen Grafana as a fallback option, since that one also seems to be a good fit for my project.


### DOT Framework for Monitoring

**Best good and bad practices:**

I've explored various monitoring tools such as Helios, Grafana, etc. I've evaluated their use and relevancy in my context and app structure. This involves understanding the pricing policies and installation processes of each tool.

### References for Monitoring
- [8] Tools for Monitoring Resources. (2024, 19 maart). Kubernetes. https://kubernetes.io/docs/tasks/debug/debug-cluster/resource-usage-monitoring/
- [9] Resource metrics pipeline. (2023, 2 augustus). Kubernetes. https://kubernetes.io/docs/tasks/debug/debug-cluster/resource-metrics-pipeline/
- [10] Prabhanga, B. (2023, 4 juli). Top 10 Kubernetes Monitoring Tools | Cloud Native Daily. Medium. https://medium.com/cloud-native-daily/kubernetes-monitoring-tools-584d82c94185
- [11] Gethelios. (2024, 17 januari). Applied observability platform that provides actionable security and observability insights. gethelios.dev. https://gethelios.dev/
- [12] Kubernetes. (z.d.). New Relic. https://newrelic.com/instant-observability/kubernetes