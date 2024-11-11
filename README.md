# Overview

This project involves the containerization and deployment of a full-stack YOLO e-commerce application using Docker. The application consists of three services:
- A **frontend** (built with React and served using NGINX)
- A **backend** (Node.js API)
- A **MongoDB** database

Docker is used to build and manage these services, while Docker Compose orchestrates and networks them together in a multi-container environment.

# Requirements

Ensure you have the following installed:

- [Vagrant](https://www.vagrantup.com/downloads) - for setting up and managing virtualized development environments
- [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html) - for configuring the VM and provisioning services 
- [Docker](https://docs.docker.com/engine/install/) - for containerization

# How to Launch the Application
## Option 1: Using Docker only

This option is ideal if you have Docker installed locally and don’t need a virtual machine.

1. **Clone the Repository:**
   ```bash
   git clone git@github.com:ChepkemoiPeris/yolo.git
   cd yolo

2. **Start the Application:**
    ```bash
   docker compose up 

This command will:

Pull the images for the frontend and backend from the repository.
Set up networking between the services.
Persist data in MongoDB using Docker volumes.

3. **Access the Application:** 
    - The frontend will be accessible at: http://localhost:80
    - The backend will be running at: http://localhost:5000 

4. **Shutting Down the Application:** 
    ```bash
    docker compose down

## Option 2: Using Vagrant and Ansible

This option is ideal if you want to automate the provisioning of a virtual environment for Docker using Vagrant and Ansible.

1. **Clone the Repository:**
   ```bash
   git clone git@github.com:ChepkemoiPeris/yolo.git
   cd yolo

2. **Start Vagrant to Provision the Environment:**
    ```bash
   vagrant up --provision

This command will:

create a virtual machine and configure Docker (including setting up network and volume), mongo, and other dependencies using Ansible.

3. **Access the Application:** 
    - The frontend will be accessible at: http://localhost:8080
    - The backend will be running at: http://localhost:5000 

4. **Shutting Down the Application:** 
To completely remove the Vagrant environment:
    ```bash
    vagrant destroy

# Screenshot of Deployed Docker Images
![Alt text](image.png)
    
# Troubleshooting

1. **View Running Containers:**
    ```bash
   docker ps 

2. **View Container Logs:**
    ```bash
   docker logs <container_name> 


# Deploying on Kubernetes
If you want to run the application on Kubernetes for scalability and production-like environments, you can use this option. You can test using minikube locally or deploy to cloud.
Kubernetes manifest files are found on k8s/ folder.
1. **Clone the Repository:**
   ```bash
   git clone git@github.com:ChepkemoiPeris/yolo.git 
    
2. **Navigate to Kubernetes YAML Files:**
Go to the k8s folder, where your Kubernetes deployment files are stored:

   ```bash 
   cd yolo/k8s
```
3. **Apply the Kubernetes Configurations:**
Use kubectl to apply the configurations. This will create services, config maps, and deployments as defined in the .yaml files:
```bash
kubectl apply -f .
```
This will:

- Create the necessary deployments for the frontend and backend.
- Create the statefulset for the MongoDB.
- Create services to expose the frontend and backend.
- Create backend and frontend config maps for configuration management.

4. **Get the External IP**
```bash
kubectl get svc frontend-service

```
This will display the external IP assigned to the frontend. It might take a few moments for the LoadBalancer to be provisioned. 
You will not get this if you are using minikube, you can skip this step.

5. **Access the Application**
Once you have the external IP, you can access the application in your browser:

If you are using minikube you can access application using: minikube service frontend-service
If you are on live server you can use:  http://<external-ip>:80

6. **Shutting Down the Application**
To delete the Kubernetes resources (services, deployments, etc.), run:

```bash
kubectl delete -f .

```

# Hosted Website
You can access the live version of this application at: http://34.30.39.228/

This link points to the production deployment of the application, it was deployed on Google Kubernetes Engine(GKE) cluster using GCP.

