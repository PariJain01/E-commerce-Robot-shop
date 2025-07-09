# 🛒 Stan's Robot Shop - Microservices Deployment on Azure Kubernetes Service

This project demonstrates deploying the **Stan's Robot Shop** microservices e-commerce application on **Azure Kubernetes Service (AKS)** with container images stored in **Azure Container Registry (ACR)** and orchestrated via **Helm**.

---

## 🚀 Project Overview

Stan’s Robot Shop is a sample microservices application composed of:
- **Frontend:** Node.js, AngularJS
- **Backend services:** Java (Spring Boot), Python (Flask), Go, PHP
- **Databases & Messaging:** MongoDB, MySQL, Redis, RabbitMQ

**Goal:** Learn containerization, orchestration, and cloud deployment on Azure.

---

## 🧰 Prerequisites

- Azure Subscription
- Azure CLI installed
- Docker installed
- Helm installed
- Kubernetes CLI (`kubectl`) installed

---

## 🛠️ Steps to Deploy

### 1️⃣ Clone This Repository

```bash
git clone https://github.com/PariJAin01/robot-shop.git
cd robot-shop/K8s/helm
```

---

### 2️⃣ Create Azure Container Registry (ACR)

```bash
az acr create --resource-group <ResourceGroup> --name <ACRName> --sku Basic
```

Example:

```bash
az acr create --resource-group RobotShopRG --name robotshopacr --sku Basic
```

---

### 3️⃣ Build and Push Docker Images

*(You can build images locally or use provided images.)*

Example build & push for one service:

```bash
docker build -t robotshopacr.azurecr.io/robot-shop/rs-web:1.0 ./path/to/web
docker push robotshopacr.azurecr.io/robot-shop/rs-web:1.0
```

Repeat for each microservice.

Verify images:

```bash
az acr repository list --name robotshopacr --output table
```
![ACR Repositories ](/sceenshots/ACR-Repo.png)
---

### 4️⃣ Create AKS Cluster

```bash
az aks create \
  --resource-group RobotShopRG \
  --name robotshop-aks \
  --node-count 2 \
  --enable-addons monitoring \
  --generate-ssh-keys
```
![AKS Cluster ](/sceenshots/AKS-Cluster.png)
---

### 5️⃣ Attach ACR to AKS

```bash
az aks update \
  --name robotshop-aks \
  --resource-group E-CommerceRG \
  --attach-acr robotshopacr
```

---

### 6️⃣ Connect kubectl to AKS

```bash
az aks get-credentials \
  --resource-group E-CommerceRG \
  --name robotshop-aks
```
![Merge AKS](/sceenshots/mergedToLocal.png/)


Verify connection:

```bash
kubectl get nodes
```
![Running Nodes](/sceenshots/Nodes.png)


---

### 7️⃣ Install the Helm Chart

Update your `values.yaml` to reference your ACR images:

```yaml
image:
  repo: robotshopacr.azurecr.io/robot-shop
  version: 1.0
  pullPolicy: IfNotPresent
```

Install:

```bash
helm install robot-shop .
```

---

### 8️⃣ Verify Deployment

```bash
kubectl get pods
```
![Running Pods](/sceenshots/Running%20Pods.png)

All pods should be `Running`.


---

## 📂 Project Structure

```
robot-shop/
├── K8s/
│   └── helm/
│       ├── Chart.yaml
│       ├── values.yaml
│       └── templates/
├── src/
├── azure-pipelines.yml
├── README.md
```

---

# 📸 Screenshots


### 1️⃣ Screenshot of AKS pods
![Running pods](/sceenshots/Pods.png)

### 2️⃣ Screenshot of service
![AKS services](/sceenshots/AKS_service.png)

### 3️⃣ Screenshot of the Pipeline Success
![Application UI](/sceenshots/Pipeline-success.png)

### 4️⃣ Screenshot of the access app
![Application UI](/sceenshots/Access%20app.png)

### 5️⃣ Screenshot of the application UI
![Application UI](/sceenshots/Application%20UI.png)

### 6️⃣ See more [Screenshots](/sceenshots/)

### 7️⃣ See my [azure-pipeline.yml](azure-pipeline.yml)
---

## Credits

Original application: [Instana Robot Shop](https://github.com/instana/robot-shop)

---

## 🙋‍♂️ Author 

Pari Jain

Azure DevOps & Kubernetes Enthusiast
