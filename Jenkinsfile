3. Project Tasks 
Task 1: Repository Setup (3 Marks)  
In this task, you will prepare two repositories to manage the project's code and configuration: 
1. Fork the AWS Sample Node.js Web Application Repository (1 Marks): 
o Fork this repository: https://github.com/aws-samples/aws-elastic-beanstalk
express-js-sample in your personal Github.  
o This repository contains a sample Node.js web application. 
2. Create a New Repository for Docker Compose (2 Marks): 
o In your GitHub account, create a new repository named Project2
Compose. 
o This repository will be used to store the Docker Compose configuration and 
related files for running Jenkins. 
Task 2: Container and Jenkins Configuration (32 Marks)                     
In this task, you will set up a Dockerized environment using Docker Compose, with Jenkins 
running inside a container. You may find some hint from here (Please refer to docker-in
jenkins-in-docker.pdf can be available in Blackboard (under Assessment 2 section) in case the 
above link is not working). 
1. Set Up Docker Compose for Jenkins and DinD (30 Marks): 
a. Create a Docker Compose file that configures: 
i. 
A Docker-In-Docker (DinD) container that allows Jenkins to run Docker 
commands. 
ii. 
A Jenkins container that uses the DinD container to run a Jenkins 
pipeline. 
2. Push Docker Compose Configuration to GitHub (2 Marks): 
a. Commit and push the Docker Compose file to the `main` branch of the 
`Project2-Compose` repository. 
b. Include comments in the file to explain each service and volume configuration. 
Task 3: Jenkinsfile Creation and Security Integration (30 Marks) 
1. Create Jenkinsfile for CI/CD Automation (18 marks): 
a. In your forked `aws-elastic-beanstalk-express-js-sample` repository, create a 
`Jenkinsfile` and store it in the root of the repo.  
b. The `Jenkinsfile` should: 
i. 
Use Node 16 Docker image as the build agent. 
ii. 
Include steps for installing dependencies using `npm install -
save`. 
Note: If you are not sure about Jenkinsfile syntax, check out 
https://github.com/darinpope/jenkins-example-docker/blob/main/Jenkinsfile-1 and 
https://www.jenkins.io/doc/tutorials/build-a-node-js-and-react-app-with-npm/  . 
2. Implement Security Practices in Jenkinsfile (10 Marks): 
a. Integrate security tool like Snyk to scan for vulnerabilities in project 
dependencies.  
b. Ensure that the pipeline halts if critical vulnerabilities are detected during the 
build or test phases. 
3. Push Jenkinsfile to GitHub (2 Marks): 
a. Commit and push the Jenkinsfile to the `main` branch of your forked `aws
elastic-beanstalk-express-js-sample` repository.  
Task 4: Pipeline Setup and Logging (20 Marks) 
1. Jenkins Pipeline Setup (12 Marks): 
a. In
 Jenkins,
 create
 a 
new
 Pipeline
 project 
named 
StudentID_Project2_pipeline (replace StudentID with your 
actual student ID). You may get help from here. 
b. Configure the pipeline to use the Jenkinsfile from your forked repository by 
selecting the Pipeline script from SCM option. 
2. Enable Logging and Monitoring (5 Marks): 
a. Set up Jenkins to store logs and ensure that logs are easily accessible (such as, 
in case of failure). 
b. Ensure the logging includes detailed information about builds, tests, and 
security scans. 
3. Run and Verify the Pipeline (3 Marks): 
a. Ensure that the pipeline runs successfully, executing all steps without errors. 
b. Take screenshots of the pipeline execution and logs to demonstrate successful 
completion. 
