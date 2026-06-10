# 🚀 Spring Boot on Kubernetes (Minikube on AWS EC2)

## 📌 Overview

This project demonstrates end-to-end deployment of a **Spring Boot application with MySQL database** on a **Kubernetes cluster running via Minikube on an AWS EC2 instance**.

It includes:
- EC2 provisioning
- Docker image creation
- Kubernetes deployments (Spring Boot + MySQL)
- Persistent storage (PV & PVC)
- Service exposure using Kubernetes networking
- Port forwarding for external access

---

## 🧱 Tech Stack

- Spring Boot
- Docker
- Kubernetes (Minikube)
- AWS EC2
- MySQL
- Docker Hub

---

## ⚙️ Infrastructure Setup (EC2)

### Update system
```bash
sudo apt update -y
sudo apt upgrade -y
```

### Install Docker
```bash
sudo apt install docker.io -y
sudo systemctl enable docker
sudo systemctl start docker
sudo usermod -aG docker ubuntu
newgrp docker
```

### Install kubectl
```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl
sudo mv kubectl /usr/local/bin/
```

### Install conntrack
```bash
sudo apt install conntrack -y
```

### Install Minikube
```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
```

---

## 🚀 Start Kubernetes Cluster

```bash
minikube start --driver=docker
kubectl get nodes
```

---

## 📦 Build & Push Docker Image

### Build Spring Boot JAR
```bash
mvn clean package
```

### Build Docker Image
```bash
docker build -t <dockerhub-username>/springboot-app:v1 .
```

### Login & Push
```bash
docker login
docker push <dockerhub-username>/springboot-app:v1
```

---

## ☸️ Kubernetes Deployment

### Apply MySQL resources
```bash
kubectl apply -f mysql-pvc.yaml
kubectl apply -f mysqldeployment.yaml
kubectl apply -f mysqlservice.yaml
```

### Apply Spring Boot app
```bash
kubectl apply -f springboot-deployment.yaml
kubectl apply -f springboot-service.yaml
```

### Verify resources
```bash
kubectl get pods
kubectl get svc
kubectl get pvc
kubectl get pv
```

---

## 🌐 Access Application

### Port Forwarding
```bash
kubectl port-forward service/springboot-service 8080:8080
```

### Open in browser
```
http://<EC2-PUBLIC-IP>:8080
```

---

## 💾 Database Persistence

- MySQL uses Persistent Volume (PV) & Persistent Volume Claim (PVC)
- Data remains intact even if pod restarts or redeploys

---

## 🔁 Scaling Application

```bash
kubectl scale deployment springboot-deployment --replicas=3
kubectl get pods
```

---

## 🔄 Rolling Updates

```bash
docker build -t <dockerhub-username>/springboot-app:v2 .
docker push <dockerhub-username>/springboot-app:v2

kubectl set image deployment/springboot-deployment \
springboot-app=<dockerhub-username>/springboot-app:v2

kubectl rollout status deployment/springboot-deployment
```

---

## ⏪ Rollback

```bash
kubectl rollout undo deployment/springboot-deployment
```

---

## 📊 Monitoring

```bash
kubectl get all
kubectl describe pod <pod-name>
kubectl logs <pod-name>
```

---

## 🧹 Cleanup

```bash
kubectl delete -f springboot-deployment.yaml
kubectl delete -f springboot-service.yaml
kubectl delete -f mysqldeployment.yaml
kubectl delete -f mysqlservice.yaml
kubectl delete -f mysql-pvc.yaml

minikube stop
minikube delete
```

---

## 📈 Learning Outcomes

- Docker containerization
- Kubernetes deployments & services
- Persistent storage (PV/PVC)
- Networking & port forwarding
- Minikube on AWS EC2
- Real-world DevOps workflow

---

## 👨‍💻 Author

DevOps Learning Project – Spring Boot Kubernetes Deployment
