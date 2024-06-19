# CI/CD

For my CI/CD pipeline, I use Github Actions. The ease of use is one of the main reasons I chose Github to host my code. The CI/CD pipeline is a series of jobs that's executed when a push is made to the repository.

## Branches

Each repository has two branches that have a CI/CD pipeline attached to them. Each branch pipeline has a different responsibility that is executed when a push is made to the branch.

### Dev Branch

The dev branch is the branch that is used for development. The pipeline for the dev branch is responsible for multiple jobs that are all executed but wait on each other to finish before moving on to the next job. The jobs that are executed are:

Build
: This job is responsible for building the application and running the tests in the application. This can either be a frontend or backend application.

Push_to_Dockerhub
: This job is responsible for building and pushing a docker image to Dockerhub. This job is only executed if the build job is successful.

Deploy_to_Netlab
: This job is responsible for deploying the application to the Netlab server, which is a test environment. This job is only executed if the Push_to_Dockerhub job is successful.

ZAP_scan (Only present for Frontend)
: This job is responsible for running the ZAP scan on the application, which checks for security vulnerabilities. This job is only executed if the Deploy_to_Netlab job is successful.

Artillery (Only present for API-Gateway)
: This job is responsible for running the artillery load tests on the application. This job is only executed if the Deploy_to_Netlab job is successful.

If any of the jobs fail, the pipeline will stop and the developer will be notified of the failure through email.

![Github-Actions-Flow-Dev.png](Github-Actions-Flow-Dev.png)

The above diagram shows the flow of the pipeline for the dev branch. The lines between the blocks show the order in which the jobs are executed and a dependency on the previous job.

### Main Branch

The main branch is the branch that is used for production. The pipeline for the main branch is responsible for multiple jobs that are all executed but wait on each other to finish before moving on to the next job. The jobs that are executed are:

Build
: This job is responsible for initiating a code analysis on the codebase using SonarCloud.

Deploy_to_Azure
: This job is responsible for deploying the application to my Azure kubernetes environment, which is a production environment. This job is only executed if the Build job is successful.

Also here, if any of the jobs fail, the pipeline will stop and the developer will be notified of the failure through email.

![Github-Actions-Flow-Main.png](Github-Actions-Flow-Main.png)

The above diagram shows the flow of the pipeline for the main branch. The lines between the blocks show the order in which the jobs are executed and a dependency on the previous job.