# Testreport


## Introduction
This document outlines the testing methodologies employed during the development of the project. The goal of these tests is to ensure the robustness, reliability, and performance of the application.

## Methods of Testing

### Component Testing
Component testing, focuses on verifying the functionality of individual components or modules of the software. Each component is tested in isolation to ensure it behaves as expected.

**Objectives:**

_Component in this case refers to a single service_

- Verify the functionality of individual components.
- Ensure each component performs as intended.
- Identify and fix defects at an early stage.

**Tools used:**

- NUnit (for C# components)

#### Test Cases - Post Service

The following test cases were created to test the endpoints within the Post Service component.

**Test Case 1: Verify GetAll Endpoint**

Objective: Verify that the GetAll endpoint returns all the items in the database.

Endpoint: `GET /api/posts`

Steps:
1. Post items to the database.
2. Call the GetAll endpoint.

Results:

| 	                     | Expected result  	  | Actual result  	    |
|-----------------------|---------------------|---------------------|
| Statuscode 	          | `200`	              | 	`200`              |
| Headers Content-Type	 | 	`application/json` | 	`application/json` |
| Posts Count	          | 3	                  | 3	                  |

**Test Case 2: Verify CreatePost Endpoint**

Endpoint: `POST /api/posts`

Body:
- Title - `string`
- Content - `string`
- UserId - `string`

Preconditions: The database is empty.

Steps:
1. Retrieve a dummy object.
2. Call the CreatePost endpoint with the dummy object.
3. Check the object that's returned.

Results:

| 	                     | Expected result  	                     | Actual result  	                       |
|-----------------------|----------------------------------------|----------------------------------------|
| Statuscode 	          | `200`	                                 | 	`200`                                 |
| Headers Content-Type	 | 	`application/json`                    | 	`application/json`                    |
| Posts Count	          | 1	                                     | 1	                                     |
| Post dummy Title      | `f5808aaa-823e-4c5d-8983-ed6fc7f26344` | `f5808aaa-823e-4c5d-8983-ed6fc7f26344` |

**Test Case 3: Verify GetByID Endpoint**

Objective: Verify that the GetByID endpoint returns the correct post given the ID.

Endpoint: `GET /api/posts/{id}`

Steps:
1. Create a post to populate the database.
2. Call the GetByID endpoint with the post's ID.

Results:

|                      | Expected result            | Actual result              |
|----------------------|----------------------------|----------------------------|
| Statuscode           | `200`                      | `200`                      |
| Headers Content-Type | `application/json`         | `application/json`         |
| Post ID              | `664e524fa84abaa969666a1e` | `664e524fa84abaa969666a1e` |

**Test Case 4: Verify Remove Endpoint**

Objective: Verify that the Remove endpoint removes the post with the specified ID.

Endpoint: `DELETE /api/posts/{id}`

Steps:
1. Create a post to populate the database.
2. Call the Remove endpoint with the post's ID.

Results:

|                      | Expected result | Actual result |
|----------------------|-----------------|---------------|
| Statuscode           | `NotFound`      | `NotFound`    |

**Test Case 5: Verify Update Endpoint**

Objective: Verify that the Update endpoint updates the post with the specified ID.

Endpoint: `PUT /api/posts/{id}`

Body:

- Title - `string`
- Content - `string`

Steps:
1. Create a post to populate the database.
2. Call the Update endpoint with the post's ID and a new object.

Results:

|            | Expected result | Actual result   |
|------------|-----------------|-----------------|
| Statuscode | `200`           | `200`           |
| New title  | `Updated Title` | `Updated Title` |


### Load Testing

Load testing is used to determine how the system behaves under various loads. It helps identify bottlenecks and performance issues that may arise when the system is under stress.

**Objectives:**

- Determine the system's performance under different loads.
- Identify performance issues.
- Ensure the system can handle the expected load.

**Tools used:**

- Artillery.io
- Postman

#### Test Cases - Load Testing

The following test cases were created to test the performance of the application under different loads.

**Test Case 1: Artillery.io multiphase test (75 users)**

Objective: Test the performance of the application under varying loads.

Artillery.io Phases:

During load testing, I use Artillery.io to simulate different levels of load on my application. I define different phases of load to simulate different usage scenarios. Here are the phases:

- Warm up phase

  - Duration: 30 seconds
  - Arrival rate: Starts with 1 virtual user
  - Ramp up to: 5 virtual users

This phase is used to warm up the application, starting with a low load and gradually increasing.

- Ramp up load

  - Duration: 60 seconds
  - Arrival rate: Starts with 5 virtual users
  - Ramp up to: 40 virtual users

In this phase, we increase the load on the application to simulate a growing number of users.

- Spike phase

  - Duration: 30 seconds
  - Arrival rate: Starts with 40 virtual users
  - Ramp up to: 75 virtual users

All of these users will send requests to the application at the same time, simulating a sudden spike in traffic.

Results:

| 	                   | Expected result  	        | Actual result  	   |
|---------------------|---------------------------|--------------------|
| Max Responsetime 	  | ` < 75ms`	                | 	`35 ms`           |
| Amount of replicas	 | 	more if deemed necessary | 	Was not necessary |


Max/Min response time:

When looking at the below graph from the Artillery.io test, I can see that the response time has stayed very low throughout the test. Only in the beginning did the response time spike, but that always happens only in the first few seconds of the test. After that the response time stayed very low, even with 75 virtual users.

![Artillery_Loadtest_1.png](Artillery_Loadtest_1.png)

_Right click image -> click `Open image in new tab` to view in fullscreen_

Users/P95 response time:

When looking at a graph of the virtual users and the 95th percentile response time, it shows that, even though the users increased in amount, the response time stayed very consistent throughout.

`95p`: The 95th percentile response time is the time it takes for 95% of the requests to be processed.

![Artillery_Loadtest_1_95p.png](Artillery_Loadtest_1_95p.png)

_Right click image -> click `Open image in new tab` to view in fullscreen_
