# jenkins-docker-cicd
Automated CI/CD pipeline using Jenkins and Docker to deploy a web application on AWS EC2.
<h2><b>Simple CI/CD Pipeline: Jenkins,Docker & AWS.</b></h2>
This project explain how to automate web deplyments. when I change my code on Github, Jenkins
automatically builds a new Docker image and Deploy it to an AWS EC2 instance.


<br>
<h3>**Step by step project implementation**</h3>
<br>
Step 1: Application Development (Local)
* I started by writing simple HTML code in notepad on my local machine
* The code was a basic web page to show that the deployment is working
* I saved this file as index.html

Step 2 : Server Setup
* Launched an AWS EC2 Instance (Ubuntu).
* Installed Jenkins and Docker on the server.
* Configured Security groups to allow traffic on port 80(Web) and port 8080(Jenkins)

Step 3 : Dockerization
* Created a Dockerfile to use Nginx as a base image
* Used the COPY command to move my index.html from the server into the nginx folder inside the container

Step 4 : Automation with Jenkins
* Created a Freestyle/Pipeline project in Jenkins.
* Linked it to my Github Repository.
* Enabled Poll SCM (*****) so jenkins checks for code changes every minute

Step 5 : Docker Hub Integration
* Created personal Access Tocken (PAT) on Docker Hub for secure login
* Configured the pipeline to Tag the image and Push it to sourabhpj94/yumgit:latest

Step 6 : Final Deployment
* Stop and remove any old containers(docker rm -f)
* Pull the latest image from Docker Hub
* Run a new container on Port 80


# Technical Implementation & Commands
**1. Setting up Docker and Jenkins on AWS EC2
First, I updated the server and installed the necessary tools to run containers and automation.
Update the package list**
sudo apt update

**2. Install Docker <br>
sudo apt install docker.io -y

** Provide Docker permissions to the current user
sudo usermod -aG docker $USER && newgrp docker

** Install Java (Required for Jenkins)
sudo apt install openjdk-17-jdk -y

 **Install Jenkins on Ubuntu**
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt update
sudo apt install jenkins -y

3.** Manual Testing: Build & Push Image**
​Before automating the process, I manually verified that the image was building and pushing correctly.
**Build the Docker image from Dockerfile
docker build -t my-app .

**Tag the image for Docker Hub
docker tag my-app sourabhpj94/yumgit:latest

**Push the image to the repository
docker push sourabhpj94/yumgit:latest

4. **Automated Deployment via Jenkins Pipeline
​These commands were used inside the Jenkins Pipeline script to ensure a smooth deployment without conflicts.
 Stop and remove the old container to free up Port 80
docker rm -f my-container || true

** Run the new container using the latest image from Docker Hub
docker run -d --name my-container -p 80:80 sourabhpj94/yumgit:latest

