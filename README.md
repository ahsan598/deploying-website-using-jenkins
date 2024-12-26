# Jenkins Project to deploy static website


Jenkins is a popular open-source automation server that helps automate various tasks in the software development process, including building, testing, and deploying software. Github is a code repository that provides version control and collaboration tools, and Sonarqube is a code quality management tool that helps analyze and manage code quality in software projects. Docker is a containerization platform that helps simplify the deployment of software applications.

First, we will create an AWS EC2 instance and install Jenkins on it. We will then set up a Github repository and integrate it with Jenkins. Next, we will configure Sonarqube to analyze code quality in our Github repository. Finally, we will create a Docker image of our application and deploy it using Jenkins.


### Process Flow:

![Project Diagram](https://github.com/ahsan598/deploying-website-using-jenkins/blob/main/JenkinsSonarqubeDocker.png)


The below steps will outline the process of deploying a website using Jenkins, Docker, SonarQube, GitHub, and EC2. By following these steps, we can set up a continuous integration and continuous deployment (CI/CD) pipeline that automates the entire deployment process, from code commit to production deployment. This ensures a streamlined, efficient, and reliable deployment pipeline for our web application.

### Implementation steps:

##### 1. Setting Up the Environment

1.1. Jenkins Installation on EC2
- Launch EC2 Instance:
- Use an Amazon Linux 2 or Ubuntu instance.
- Configure security groups to allow HTTP (port 80), HTTPS (port 443), Jenkins (port 8080) and SonarQube (9000).

1.2. Jenkins Installation
- Install Java:
```sh
sudo yum update -y
sudo yum install java-1.8.0-openjdk-devel -y
```

- Install Jenkins
```sh
wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
sudo yum install jenkins -y
sudo systemctl start jenkins
sudo systemctl enable jenkins
```

1.3. Docker Installation
- Install Docker:
```sh
sudo yum install docker -y
sudo systemctl start docker
sudo systemctl enable docker
sudo usermod -aG docker jenkins
sudo systemctl restart jenkins
```

1.4. SonarQube Installation
- Pull SonarQube Docker Image:
```sh
docker pull sonarqube
```

- Run SonarQube Container:
```sh
docker run -d --name sonarqube -p 9000:9000 sonarqube
```

##### 2. Configuring GitHub Repository

2.1. Create a Repository:
- Create a new repository on GitHub for the web application.
- Clone Repository:

```sh
git clone https://github.com/your-username/your-repository.git
cd your-repository
```

##### 3. Integrating Jenkins with GitHub

3.1. Install GitHub Plugin:
- Go to Jenkins Dashboard > Manage Jenkins > Manage Plugins > Available.
- Search for "GitHub Integration Plugin" and install it.

3.2.  Configure GitHub Webhook:
- Go to your GitHub repository settings.
- Navigate to Webhooks and add a new webhook.
- Set the payload URL to http://<your-jenkins-server-ip>:8080/github-webhook/.


##### 4. Jenkins Pipeline Configuration

4.1. Create Jenkins Pipeline Job:
- Create a new pipeline job in Jenkins.
- Configure the pipeline script to pull the code from GitHub and build it.
- Pipeline Script (Jenkinsfile)  is added in repository

#####  5. Dockerizing the Application

5.1.  Dockerfile:
- Dockerfile  is added in repository

- Build Docker Image:

```sh
docker build -t your-docker-image-name .
```

##### 6. SonarQube Quality Gate

6.1. SonarQube Configuration:
- Access SonarQube at http://<your-ec2-ip>:9000.
- Set up a new project and generate a token.
- Add the token and project key in Jenkinsfile.

##### 7. Deployment to EC2

7.1. EC2 Configuration:
- Ensure the EC2 instance has Docker installed.
- Ensure security groups allow traffic on the necessary ports (Eg: 8081).

7.2. Deploy Application:
- Use Jenkins to deploy the Docker image to the EC2 instance.