Spring Boot on Kubernetes (Minikube on EC2)

This project demonstrates deploying a Spring Boot application with a database on a Kubernetes cluster using Minikube running on an AWS EC2 instance.

🚀 Tech Stack
Spring Boot
Docker
Kubernetes (Minikube)
AWS EC2
MySQL
Docker Hub

📌 Architecture
EC2 instance hosts Minikube cluster
Spring Boot app runs as Kubernetes Deployment
MySQL runs as separate pod with persistent storage
Docker images pulled from Docker Hub
Services exposed via ClusterIP / Port-forward

Setup Steps
1. Install Dependencies on EC2
sudo apt update -y
sudo apt install docker.io conntrack -y

Install kubectl:
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl
sudo mv kubectl /usr/local/bin/

Install Minikube:
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube

2.Start Kubernetes Cluster
minikube start --driver=docker
kubectl get nodes

3.Build & Push Docker Image
mvn clean package
docker build -t <dockerhub-username>/springboot-app:v1 .
docker push <dockerhub-username>/springboot-app:v1

4. Deploy to Kubernetes
kubectl apply -f mysql/
kubectl apply -f springboot/

Check status:
kubectl get pods
kubectl get svc

5. Access Application
Port forward:
kubectl port-forward service/springboot-service 8080:8080
Open:
http://<EC2-PUBLIC-IP>:8080

6.Kubernetes Dashboard
minikube addons enable dashboard

📦 Features
Containerized Spring Boot app
MySQL with persistent storage
Kubernetes deployments & services
External access via port forwarding
Easy scaling & updates

📈 Learning Outcome
Docker & Kubernetes
Deployment on Minikube
Service networking & storage
Real-world DevOps workflow
