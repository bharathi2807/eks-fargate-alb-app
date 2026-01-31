# EKS Fargate Flask App with ALB Ingress

This project demonstrates deploying a **Python Flask application** on **Amazon EKS using Fargate (serverless Kubernetes)** and exposing it to the internet using an **Application Load Balancer (ALB)** via **Kubernetes Ingress**.

No EC2 worker nodes were used.

---

## Architecture Overview

Client  
→ ALB (AWS Load Balancer Controller)  
→ Kubernetes Ingress  
→ ClusterIP Service  
→ Flask Pods (running on Fargate)

---

## Tech Stack

- AWS EKS (Kubernetes)
- AWS Fargate
- AWS Load Balancer Controller
- Application Load Balancer (ALB)
- Docker
- Python Flask

---

## Project Structure

```text

├── app.py # Flask application
├── requirements.txt # Python dependencies
├── Dockerfile # Container image definition
├── flask-eks.yml # Kubernetes manifests (Deployment, Service, Ingress)
├── iam_policy.json # IAM policy for ALB controller
├── README.md
├── .gitignore

```
---

## Key Kubernetes Concepts Used

- Fargate-only EKS cluster
- ALB Ingress with `target-type: ip`
- ClusterIP Service
- Kubernetes Ingress resources
- Real-world ingress debugging (HTTP vs browser behavior)

---

## How to Run (High Level)

1. Build and push Docker image to ECR
2. Create EKS Fargate cluster
3. Install AWS Load Balancer Controller
4. Apply Kubernetes manifests
5. Access app via ALB DNS

---
## Key CLI Commands Used

````md
### Create EKS Fargate Cluster
```bash
eksctl create cluster \
  --name demo-cluster \
  --region us-east-1 \
  --fargate
````

### Install AWS Load Balancer Controller

```bash
helm repo add eks https://aws.github.io/eks-charts
helm repo update

helm install aws-load-balancer-controller eks/aws-load-balancer-controller \
  -n kube-system \
  --set clusterName=demo-cluster \
  --set serviceAccount.create=false \
  --set serviceAccount.name=aws-load-balancer-controller
```

### Deploy Application

```bash
kubectl apply -f flask-eks.yml
```

### Verify Resources

```bash
kubectl get pods -A
kubectl get svc -A
kubectl get ingress -A
```
---

## What I Learned

- How ALB actually integrates with Kubernetes
- Why Fargate requires `target-type: ip`
- How traffic flows from ALB → Service → Pods
- Debugging ingress issues instead of blindly trusting YAML

---

## 👨‍💻 Author

**Jeeva Bharathi**
Aspiring DevOps / Cloud Engineer

---
