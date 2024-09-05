# Income Tax Calculator App 

Authors: John Dela Cruz, Eduar Mancera, Marceli Ciesielski
This project was submitted as a final project assignment at the mthree academy training on the 6th of September 2024.

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

## Back-End

The back-end of the application is located in a single file found in /python_scripts/app.py. This script loads the database, reads and processes the user input, updates the database and sends the processed data to the result page. 

## Docker
I created a Docker image using a Dockerfile, which allowed me to containerize the application for consistent deployment. To ensure it was working properly, I set up the container to access port 5000, which was necessary for the application’s functionality. 
Later on, I will be using Docker Hub to store my Docker image, allowing Minikube to pull the image from the remote repository for deployment in my Kubernetes environment.

## Challenges with Docker
One challenge I faced was ensuring that port 5000 was open in the AWS security groups, as the container needed to communicate through that port. To apply this change, I had to restart or stop and start the AWS instance to make sure the new settings took effect.
Another challenge was that, since the application was still being updated, I had to ensure any changes were reflected not only in the Dockerfile but also in the requirements.txt file. This was critical to making sure the right packages were installed within the container, allowing the application to run smoothly without any issues.

## How to Deploy Docker Container
Build docker image:
- docker build -t your_image_name .

Run docker container:
- docker run -d -p 5000:5000 your_image_name

##Extras
Original github link: https://github.com/ledmarceli/mthree_final_project
