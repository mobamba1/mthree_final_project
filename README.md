# Income Tax Calculator App 

Authors: John Dela Cruz

## John's Contribution 

Front-End HTML CSS files, backend Python, SQL Lite, Dcoker, Jenkins, Kubernetes, Nginx and Grafana 

## Table of Contents

- [Introduction](#introduction)
- [SRE Concept and applications](#introduction)
- [Front-End](#front-end)
- [Back-End](#back-end)
- [Docker](#docker)
- [Jenkins](#jenkins)
- [Kubernetes](#kubernetes)
- [Grafana](#grafana)
- [Testing](#testing)
- [How to run](#testing)
- [Difficulties](#testing)
- [Future improvements](#introduction)
- [Extras](#extras)

## Introduction

⁤This application offers a basic income tax calculator with an intuitive interface that makes it easy for users to input their information. ⁤⁤Once submitted, the data is processed on the server using a Python script, with the results stored in a database and then presented back to the user. ⁤⁤Although the functionality is simple, the app is built on a solid infrastructure designed to support scalability, continuous integration, and effective monitoring. 

In this project, a variety of tools and services were used to develop, deploy, and monitor the application. Python served as the core programming language, while Flask was utilized as a web framework to build a lightweight and flexible backend. Data storage was handled using SQLite, a simple and file-based database suitable for this project.

To streamline deployment, Docker was used to containerize the application, making it easier to run consistently across different environments. The containers were stored and shared through Docker Hub, allowing for easy distribution.

For continuous integration and deployment, Jenkins was set up to automate the build and deployment pipeline, ensuring code updates could be tested and deployed efficiently. A local Kubernetes environment was created using Minikube, which helped manage and orchestrate the Docker containers.

To monitor the system's performance, Grafana was implemented, providing useful insights into the application's metrics. Lastly, AWS services were used to deploy the project, taking advantage of cloud hosting to ensure scalability and reliability.

## Front-End

The front-end of the application consists of simple HTML and CSS files found in /python_scripts/templates and /python_scripts/static respectively. These two provide a basic and user-friendly interface to communicate with the user and send the information provided through an HTML form to the backend script.

My Contribution:
- Code review and changes for import modules and text.

## Back-End

The back-end of the application is located in a single file found in /python_scripts/app.py. This script loads the database, reads and processes the user input, updates the database and sends the processed data to the result page. 

My Contribution:
-  Research for JSON data Passing for Grafan

## Docker
I created a Docker image using a Dockerfile, which allowed me to containerize the application for consistent deployment. To ensure it was working properly, I set up the container to access port 5000, which was necessary for the application’s functionality. 
Later on, I will be using Docker Hub to store my Docker image, allowing Minikube to pull the image from the remote repository for deployment in my Kubernetes environment.
## Add docker file image 

Challenges with Docker:
- One challenge I faced was ensuring that port 5000 was open in the AWS security groups, as the container needed to communicate through that port. To apply this change, I had to restart or stop and start the AWS instance to make sure the new settings took effect.
- Another challenge was that, since the application was still being updated, I had to ensure any changes were reflected not only in the Dockerfile but also in the requirements.txt file. This was critical to making sure the right packages were installed within the container, allowing the application to run smoothly without any issues.

How to Deploy Docker Container
First install docker and check if its running:
- sudo systemctl status docker
Build docker image:
- docker build -t your_image_name .

Run docker container:
- docker run -d -p 5000:5000 your_image_name

## Jenkins:
I set up a Jenkins server to handle continuous integration and delivery (CI/CD), which made it much easier and faster to deploy new builds. This helped cut down on repetitive tasks, allowing us to save time on testing and releasing updates. As a result, we had more time to focus on development.
## Add image of jenkins

Challenges with Jenkins:
When I first set up Jenkins, everything worked smoothly, and I could access the web interface through the IP address and port. However, after stopping and restarting the instance, Jenkins would start very slowly. I discovered that this was due to several issues, including unclean shutdowns of the Jenkins service, old builds piling up, and plugin downloads. These factors combined to slow down Jenkins during startup.

Another issue I faced was related to permissions. Jenkins didn’t have the necessary permissions to execute tasks for Docker and Kubernetes. To resolve this, I added Jenkins to the sudo, docker, and kubernetes user groups to give it the required privileges.

To specifically resolve the problem with Kubernetes, I gave Jenkins sudo privileges by running:
- usermod -a -G sudo jenkins

Even after giving Jenkins sudo privileges, it couldn’t access certain files, like the client key for Kubernetes (kubectl). To fix this, I had to adjust the permissions for Minikube by running:
- sudo chmod -R 755 /home/ubuntu/.minikube

I verified the changes using:
- sudo -u jenkins ls -l /home/ubuntu/.minikube/profiles/minikube/client.key

How to start Jenkins server:
1. Install Jenkins: Start by installing Jenkins on your instance.
2. Start the Jenkins service: After installation, make sure to start the Jenkins service and ensure that port 8080 is open in your AWS Security Groups. To apply the changes, remember to reboot or stop/start the instance.
   - sudo systemctl status jenkins
   - sudo systemctl stop jenkins
   - sudo systemctl start jenkins
3. Access Jenkins: Once Jenkins is running, you can access the Jenkins configuration page by entering your public IP followed by port 8080 in your web browser.
4. Install Plugins: On the Jenkins configuration page, skip unnecessary steps and install only the recommended plugins. After that, create your Admin user.
5. Configure Plugins for Docker and Kubernetes: Once Jenkins is up, go to Configure Jenkins and navigate to the Plugins section. Download the necessary plugins for Docker and Kubernetes.
6. Apply Configuration: After configuring the plugins, restart Jenkins to apply the changes.
7. Test Jenkins User Before Running Builds: Before running builds with a Jenkinsfile, it’s recommended to test the commands in the Jenkinsfile by switching to the Jenkins user and verifying that everything works as expected. You can switch to the Jenkins user with:
   - sudo su jenkins
8. Follow Troubleshooting Steps: If you encounter any issues, refer to the "Challenges with Jenkins" section for guidance.

## Kubernetes
Kubernetes was used to deploy Docker images of the Flask application. By using Minikube along with Docker Hub, I was able to easily orchestrate the services, seamlessly pulling the latest image from Docker Hub for deployment.

How to deploy images with kubemini:

1. First, ensure that Minikube is installed on your system.
2. To verify if Minikube is running:
   - minikube status
3. If Minikube is not running, start it with:
   - minikube start
4. Stop or Delete Minikube (When required):
   - minikube stop
   - minikube delete
5. Initially, no pods or services will be running. To start them, apply your Kubernetes deployment and service configurations:
   - kubectl apply -f deployment.yaml
   - kubectl apply -f service.yaml
6. After applying the configurations, check that your pods and services are running:
   - kubectl get pods
   - kubectl get services
7. The service is where your Flask app will be running, and it will use a NodePort for Minikube services. To verify that the Flask app is running:
   - Check Flask App with curl
   - minikube ip
   - curl minikube_ip:nordport
8. Once you've confirmed the correct Minikube IP and NodePort for your Flask app, install Nginx to act as a reverse proxy. This will route external traffic to your Flask app running in Minikube.
9. To configure Nginx, edit the default configuration file:
    - vim /etc/nginx/sites-available/default
    - Update the configuration with your instance's public IP and Minikube's IP/NodePort:
    - server_name <your-instance-public-ip>;  # Use the public IP of your instance
    - proxy_pass http://<minikube-ip>:<NodePort>;  # Use Minikube's IP and the NodePort of your service
10. Check Nginx Configuration and Reload:
    - sudo nginx -t
    - sudo systemctl reload nginx
11. After configuring Nginx, your Flask app should be accessible via the instance's public IP on port 5000:
    - http://instance_ip:5000



   


Challenges:
- Initially, I tried to orchestrate the application using a local Docker image, but I ran into difficulties. As a result, I decided to use the more standard approach of pulling the image from Docker Hub.
- Another challenge was that Minikube only supports local orchestration, which made it difficult for the Flask app to be accessible from the internet. I resolved this by using Nginx as a reverse proxy. Nginx listened on a specific port and forwarded the connection to the internal port where the Kubernetes service running the Flask app was hosted.
- Running Minikube alongside a Jenkins server on a t2.micro instance resulted in poor performance for both services, as Minikube requires a minimum of 2 CPU cores to function effectively. Additionally, Docker needed to be running to build and push images to Docker Hub. To resolve this, I communicated to the team that upgrading the EC2 instance was essential for running all these tools smoothly. We agreed to share the cost and upgraded the instance, which significantly improved performance.
- I encountered an issue where Minikube would struggle to load the plugins properly whenever I stopped and started it again. To fix this, instead of simply stopping Minikube, I opted to delete the cluster and start fresh each time. This approach seemed more practical and closer to real-world scenarios, as it allowed the project to be quickly downloaded and set up from scratch without persistent issues.





























## Extras

Original github link: https://github.com/ledmarceli/mthree_final_project

Videos:
Jenkins build with kubernetes 

Grafana integrated with prometheus to scrap data from Jenkins/kubernetes instance

Groups Helped:
- Group 1 = sent and explained how I was able to expose kubemini services to the public ip.
- Group 6 = why might their jenkins server is slow or have crashed. 

