# Income Tax Calculator App 

Authors: John Dela Cruz

## John's Contribution 

Front-End HTML CSS files, backend Python, SQL Lite, Docker, Jenkins, Kubernetes, Nginx and Grafana 

## Table of Contents

- [Introduction](#introduction)
- [Infrastructure](#infrastructure)
- [SRE Principles](#SRE)
- [Prerequisites](#Prerequisites)
- [Front-End](#front-end)
- [Back-End](#Back-End)
- [Docker](#docker)
- [Jenkins](#jenkins)
- [Kubernetes](#kubernetes)
- [Grafana](#grafana)
- [Future Improvements](#Improvements)
- [Extras](#extras)

## Introduction

⁤This application offers a basic income tax calculator with an intuitive interface that makes it easy for users to input their information. ⁤⁤Once submitted, the data is processed on the server using a Python script, with the results stored in a database and then presented back to the user. ⁤⁤Although the functionality is simple, the app is built on a solid infrastructure designed to support scalability, continuous integration, and effective monitoring. 

In this project, a variety of tools and services were used to develop, deploy, and monitor the application. Python served as the core programming language, while Flask was utilized as a web framework to build a lightweight and flexible backend. Data storage was handled using SQLite, a simple and file-based database suitable for this project.

To streamline deployment, Docker was used to containerize the application, making it easier to run consistently across different environments. The containers were stored and shared through Docker Hub, allowing for easy distribution.

For continuous integration and deployment, Jenkins was set up to automate the build and deployment pipeline, ensuring code updates could be tested and deployed efficiently. A local Kubernetes environment was created using Minikube, which helped manage and orchestrate the Docker containers.

To monitor the system's performance, Grafana was implemented, providing useful insights into the application's metrics. Lastly, AWS services were used to deploy the project, taking advantage of cloud hosting to ensure scalability and reliability.

## Infrastructure
We hosted our project on AWS, and the images below illustrate the data flow and how the tools and AWS services interact with one another. All tools used was deplying using AWS EC2.

Image of Flow chart:

![Screenshot_2024-09-03_at_12 38 13-removebg-preview](https://github.com/mobamba1/mthree_final_project/blob/main/Mthree%20project%20pictures/flowchart.JPG)

Image of Cloud Infrastructure:

![Screenshot_2024-09-03_at_12 38 13-removebg-preview](https://github.com/mobamba1/mthree_final_project/blob/main/Mthree%20project%20pictures/Infrastructure%20VPC.JPG)

Here are the security measures I implemented:

Security Groups: I had set up security groups to allow only specific ports to communicate with our instances, limiting access and enhancing security.

IAM Users: AWS Identity and Access Management (IAM) to create user accounts for all team members, ensuring that only authorized individuals have access to the cloud environment.

PEM Keys: Generated PEM keys for SSH access, preventing unauthorized external users from accessing our instances.

Private Kubernetes Cluster: Deployed Kubernetes in a cluster using private IP addresses, preventing external communication except through Nginx port forwarding.

Jenkins Security: Secured our Jenkins server and Linux jenkins user by adding user authentication, preventing external threats from executing builds without proper authorization.

## SRE 
While building this project, we applied SRE principles to ensure that it adhered to best practices for reliability, scalability, and efficiency.
1. Embracing risk 
2. Service Level Objectives 
3. Eliminate Toil 
4. Monitor Distributed Systems 
5. Automation 
6. Release Engineering 
7. Simplicity

We achieved this by doing the following:

Embracing risk/Simplicity: We had to carefully consider the cost of using a larger instance to run both Jenkins and Kubernetes on the same machine. By evaluating which instance type offered the best performance at the lowest cost, we were able to minimize expenses. This approach ensured simplicity, allowing Kubernetes and Jenkins to work together seamlessly without overcomplicating our infrastructure.

Service Level Objective: To meet our Service Level Objectives, we chose to use an on-demand instance with sufficient performance. This ensured that our tools were available whenever needed and could handle the required tasks efficiently. We also chose to use SQLite integrated directly with the Flask app, reducing the latency between the database and the application.

Eliminating Toil/Automation: By introducing Jenkins, I was able to automate our builds, not just for live deploymnet but also for testing. This help reduce the time of testing and providing more allowance for development process. 

Monitoring Distributed Systems: We used Grafana to monitor both our instances and the application. This allowed me to create custom dashboards to track the health of the system and quickly identify any issues that needed attention.

Release Engineering: We used Git and GitHub to track project tasks and manage new releases to the production environment. Working in daily sprints, we assigned tasks, provided updates, and coordinated effectively under time pressure. This integration allowed us to communicate clearly and prioritize our work efficiently.

## Prerequisites:
Before you run the repository, follow these steps to ensure all required tools are installed and ready:

1. Update Linux machine
   -  sudo apt-get update

2. Need to download and install python:
   - sudo apt-get install python3 flask sqlite3

3. Need to download and install Docker: sudo apt-get docker
   - sudo apt-get install docker.io

4. Needs to donload and install jenkins:
   - wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add -
   - sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
   - sudo apt-get update
   - sudo apt-get install jenkins

5. Needs to download and install kubemini:
   - curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube_latest_amd64.deb
   - sudo dpkg -i minikube_latest_amd64.deb

6. Need to install nginx:
   - sudo apt install nginx




## Front-End

The front end of the app is made up of basic HTML and CSS files, which are stored in /python_scripts/templates for HTML and /python_scripts/static for CSS. These files provide a simple, user-friendly interface that lets users interact with the app and submit their information through an HTML form to the backend for processing.

My Contribution:
- Code review and changes for import modules and text.

Image of Front-End:

![Screenshot_2024-09-03_at_12 38 13-removebg-preview](https://github.com/mobamba1/mthree_final_project/blob/main/Mthree%20project%20pictures/front-end.png)

## Back-End

The back end of the app is contained in one file, located at /python_scripts/app.py. This script handles loading the database, processing the user’s input, updating the database, and then sending the processed data to the results page. For unit testing, we used Selenium to validate user inputs, ensuring that the data was stored correctly and processed without errors.

My Contribution:
-  Research for JSON data Passing for Grafana and update imports on app.py

Image of Database:

![Screenshot_2024-09-03_at_12 38 13-removebg-preview](https://github.com/mobamba1/mthree_final_project/blob/main/Mthree%20project%20pictures/database_schema.png)

## Docker
I created a Docker image using a Dockerfile, which allowed me to containerize the application for consistent deployment. To ensure it was working properly, I set up the container to access port 5000, which was necessary for the application’s functionality. 
Later on, I will be using Docker Hub to store my Docker image, allowing Minikube to pull the image from the remote repository for deployment in my Kubernetes environment.

Image of Dockerhub:

![Screenshot_2024-09-03_at_12 38 13-removebg-preview](https://github.com/mobamba1/mthree_final_project/blob/main/Mthree%20project%20pictures/Dockerhub.png)

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

Image of Jenkins pipeline:

![Screenshot_2024-09-03_at_12 38 13-removebg-preview](https://github.com/mobamba1/mthree_final_project/blob/main/Mthree%20project%20pictures/Jenkinsbuild.png)

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

Image of Kubernetes cluster:

![Screenshot_2024-09-03_at_12 38 13-removebg-preview](https://github.com/mobamba1/mthree_final_project/blob/main/Mthree%20project%20pictures/Kubernetes.png)

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

Image of nginx:

![Screenshot_2024-09-03_at_12 38 13-removebg-preview](https://github.com/mobamba1/mthree_final_project/blob/main/Mthree%20project%20pictures/nginx.png)

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

## Grafana

I used Grafana along with Prometheus to collect data from both the application and the instance it’s running on. This setup allowed me to create visual graphs that automatically update whenever new data is available.

Image of Grafana Dashboard:

![Screenshot_2024-09-03_at_12 38 13-removebg-preview](https://github.com/mobamba1/mthree_final_project/blob/main/Mthree%20project%20pictures/Grafana.png)

Challenges:
- Since we were using SQLite, Grafana couldn't access the data directly. To fix this, We added a dedicated function in app.py that generates the data. This allowed me to integrate Grafana and make it read the content from the table.
- Since our Grafana server was running on a different instance from the Jenkins/Kubernetes instance, we couldn’t get data from it. To solve this, I installed Prometheus on the Jenkins/Kubernetes instance, which allowed us to generate and capture readings from that instance.



## Improvements:
Though the project went well and we successfully integrated all the required tools, if we had more time, I would have liked to include additional tools such as Terraform, AWS Alarms, CloudWatch, Auto Scaling, and Load Balancers, along with Ansible. Combining these services would help reduce repetitive tasks, improve visibility, and better prepare for disaster recovery.

Here’s how I would implement them:

Terraform: 
I would create a Terraform configuration file that defines all the AWS resources we need, like VPC, EC2, and Security Groups. This would help us maintain and manage our cloud infrastructure. Since Terraform is cloud-agnostic, the same configuration could be used with other cloud providers like Azure or GCP. Additionally, if our cloud environment fails, we could use the Terraform configuration to quickly rebuild our services in a different account or even on a different cloud provider.

Ansible: 
Is another configuration management tool that allows you to connect to multiple instances and install packages. It would help us manage our infrastructure by ensuring all instances have the correct packages installed. I would implement this by setting up a host node for Ansible, which would connect to other instances and handle installing the necessary software and packages to keep everything running smoothly.

AWS Alarms, Cloudwatch and Auto Scaling: 
AWS Alarms, CloudWatch, and Auto Scaling work together seamlessly. AWS Alarms notify users via SNS (Simple Notification Service) when a configured threshold, like CPU usage, is triggered. CloudWatch is the service that monitors and tracks the metrics of instances, while Auto Scaling adjusts the instance's capacity, either by scaling horizontally or vertically, to ensure there are enough resources to prevent crashes. I would configure CloudWatch to monitor the metrics of the Jenkins instance and set up an alarm to notify me if it crosses a defined threshold. In case the alarm is triggered, Auto Scaling would automatically scale the instance, reducing the manual effort required to manage performance issues.

Load Blancer:
A load balancer is a tool that distributes incoming traffic across multiple servers to prevent any single server from becoming overwhelmed. It helps improve performance and reliability by balancing the load, ensuring smooth operation even during traffic spikes. For this project, I would use a load balancer specifically for the Flask app by configuring an AWS Elastic Load Balancer (ELB). This would route traffic to healthy instances running the Flask app and work with Auto Scaling to add more instances when needed. This setup ensures the Flask app handles traffic efficiently and maintains high availability.

## Extras

Original github link: https://github.com/ledmarceli/mthree_final_project

Videos:

To watch the videos, you can either download the files in /videos or click the links bellow:

Jenkins build with kubernetes:

[Jenkins](https://youtu.be/yfAG7TPbNNQ)

Update Database with Grafana Dashboard:

[Update Database](https://youtu.be/SN28P0Po7EQ)



Grafana integrated with prometheus to scrap data from Jenkins/kubernetes instance

Groups Helped:
- Group 1 = sent and explained how I was able to expose kubemini services to the public ip.
- Group 6 = why might their jenkins server is slow or have crashed. 

