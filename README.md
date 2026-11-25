# Jenkins + SonarQube CI/CD Pipeline on AWS

A CI/CD implementation for a Java web application using Jenkins, Maven, SonarQube, Docker and Apache Tomcat on AWS EC2.

## Project Overview

This project automates the build, code quality analysis and deployment of a Java web application using a Jenkins CI/CD pipeline.

The pipeline retrieves source code from GitHub, builds the application with Maven, performs SonarQube code analysis, validates the Quality Gate and deploys the application using Docker and Tomcat.

## Architecture

```text
Developer
   |
   v
GitHub Repository
   |
   v
Jenkins CI/CD
   |
   |-- Checkout
   |-- Maven Build
   |-- SonarQube Analysis
   |-- Quality Gate
   |
   v
Docker Build
   |
   v
Tomcat Container
   |
   v
Web Application (Port 8081)
```

### Technology Stack

- AWS EC2
- Ubuntu Linux
- Jenkins
- Java 17
- Maven
- SonarQube
- PostgreSQL
- Docker
- Apache Tomcat 10.1
- Git and GitHub

## Project Structure

```text
jenkins-sonarqube-ci-cd/
├── README.md
├── Jenkinsfile
├── Dockerfile
├── pom.xml
├── src/
│   └── main/
│       └── webapp/
│           └── index.jsp
└── docs/
    └── implementation-guide.docx
```

## CI/CD Pipeline Stages

1. **Checkout** – retrieves the source code from GitHub.
2. **Build** – compiles and packages the application using Maven.
3. **SonarQube Analysis** – analyzes the source code.
4. **Quality Gate** – waits for the SonarQube Quality Gate result.
5. **Docker Build & Run** – builds a Docker image and deploys the application in a Tomcat container.
6. **Validation** – confirms the pipeline stages completed successfully.

## AWS Infrastructure

The implementation uses EC2 instances for the CI/CD infrastructure.

| Component | Port |
|---|---:|
| Jenkins | 8080 |
| SonarQube | 9000 |
| Application / Tomcat host port | 8081 |
| PostgreSQL | 5432 |

PostgreSQL is used as the SonarQube database. It can be hosted on the SonarQube server for this small implementation.

## Implementation Steps

### 1. Provision AWS EC2 Instances

- Launch Ubuntu EC2 instances.
- Configure security groups.
- Install Java and required dependencies.
- Ensure sufficient disk space and memory for Jenkins and SonarQube.

### 2. Install and Configure SonarQube

- Install PostgreSQL.
- Create a dedicated database and database user for SonarQube.
- Install SonarQube.
- Configure the database connection.
- Start and validate SonarQube.

Access SonarQube:

```text
http://<SONARQUBE-IP>:9000
```

### 3. Install Jenkins

- Install Java required by Jenkins.
- Install Jenkins.
- Start and enable the Jenkins service.
- Access the Jenkins web interface.
- Complete the initial setup.
- Install required plugins.

Access Jenkins:

```text
http://<JENKINS-IP>:8080
```

### 4. Configure Jenkins Tools

Configure the following tools and plugins in Jenkins:

- Git
- Maven
- Docker
- SonarQube Scanner integration
- Pipeline
- Pipeline utilities as required

Ensure Jenkins can execute Maven and Docker commands.

### 5. Configure SonarQube Integration

- Generate a SonarQube authentication token.
- Store the token securely in Jenkins Credentials.
- Configure the SonarQube server in Jenkins.
- Configure the SonarQube webhook to notify Jenkins.

Webhook URL:

```text
http://<JENKINS-IP>:8080/sonarqube-webhook/
```

### 6. Prepare the Application

The application uses Maven WAR packaging and Java 17.

Build the application:

```bash
mvn clean package
```

The generated artifact:

```text
target/ROOT.war
```

### 7. Build the Docker Image

The Dockerfile uses Apache Tomcat 10.1 with Java 17 and deploys the generated WAR file as `ROOT.war`.

```bash
docker build -t my-tomcat:latest .
```

### 8. Deploy the Application

The Jenkins pipeline runs the Docker container:

```bash
docker run -d \
  --name tomcat \
  -p 8081:8080 \
  my-tomcat:latest
```

### 9. Run the Jenkins Pipeline

- Create a Jenkins Pipeline job.
- Configure the GitHub repository.
- Configure the Jenkinsfile pipeline.
- Run the pipeline.
- Monitor all stages.

The pipeline deploys the application only after the build, SonarQube analysis and Quality Gate stages succeed.

## Application Access

After successful deployment:

```text
http://<JENKINS-SERVER-IP>:8081/
```

## Useful Commands

### Jenkins

```bash
sudo systemctl status jenkins
sudo systemctl restart jenkins
```

### SonarQube

```bash
sudo -u sonar /opt/sonarqube/bin/linux-x86-64/sonar.sh status
```

### Docker

```bash
docker ps
docker ps -a
docker images
docker logs tomcat
```

### System Monitoring

```bash
df -h
free -h
sudo ss -lntp
```

## Security Considerations

- Restrict EC2 security group access.
- Avoid exposing PostgreSQL publicly.
- Store authentication tokens in Jenkins Credentials.
- Never commit credentials or private keys to GitHub.
- Restrict Jenkins and SonarQube access to authorized users.

## Expected Outcome

A working automated CI/CD pipeline that:

- Retrieves code from GitHub.
- Builds the Java application.
- Performs SonarQube analysis.
- Enforces the Quality Gate.
- Builds a Docker image.
- Deploys the application using Tomcat.

