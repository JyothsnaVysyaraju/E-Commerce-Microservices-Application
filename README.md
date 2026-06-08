# Production-Grade CICD Pipeline for E-Commerce Microservices AWS K8s Helm GitHub Actions ArgoCD

The pipeline leverages GitHub Actions for continuous integration and ArgoCD for continuous delivery, ensuring automated, secure, and consistent deployments on Amazon EKS (Elastic Kubernetes Service).

Each microservice is containerized with Docker, packaged and deployed using Helm charts, and managed via Kubernetes manifests following GitOps best practices. The setup includes environment-specific Helm values, automated testing, image scanning, and blue-green deployment strategies for zero-downtime releases.

Key Features:

Automated build, test, and deploy pipeline using GitHub Actions

Helm-based deployment templates for microservices

GitOps workflow with ArgoCD for version-controlled deployments

EKS cluster setup and management on AWS

Integration with three databases (PostgreSQL, MongoDB, Redis)

## Contents

- `ecommerce-ui/` — React frontend and a small Node.js backend used for serving the UI and acting as a gateway in some examples.
- `product-catalog/` — Simple product catalog service (Node.js) that returns product data.
- `product-inventory/` — Inventory API (Python) that exposes stock information.
- `order-management/` — Spring Boot application that demonstrates order processing and persistence.
- `shipping-and-handling/` — Shipping service (Go) used to calculate shipping and costs.
- `profile-management/` — Authentication/profile microservice (Node.js).
- `contact-support-team/` — Contact/support microservice (Python) providing a simple contact API.

## Build, push, run

Quick: build images, push to Docker Hub user `jyothsnav`, start DBs, run services.

1. Login to Docker Hub:

```bash
docker login
```

2. Build & push images (run from repo root):

   ```bash
   docker build -t jyothsnav/ecommerce-ui:latest ./ecommerce-ui 
   docker build -t jyothsnav/product-catalog:latest ./product-catalog 
   docker build -t jyothsnav/product-inventory:latest ./product-inventory 
   docker build -t jyothsnav/order-management:latest ./order-management 
   docker build -t jyothsnav/shipping-and-handling:latest ./shipping-and-handling 
   docker build -t jyothsnav/profile-management:latest ./profile-management 
   docker build -t jyothsnav/contact-support-team:latest ./contact-support-team
   ```
Build all the micro-services using docker to run the application locally

<img width="1967" height="734" alt="image" src="https://github.com/user-attachments/assets/03a96eb6-06c3-430e-9a21-da75adc291a5" />

3. We can start these services by using docker-compose

   run

   ```bash
   docker-compose up -d
   ```

---

# Note

## Docker Networking if running locally

To enable communication between the Product Catalog microservice and other microservices or applications, ensure that the containers are connected to the same Docker network. You can achieve this by following these steps:

1. Create a Docker network:

   ```bash
   docker network create my-network
   ```

2. Run the microservice like Product Catalog microservice container with the `--network` flag:

   ```bash
   docker run --network my-network -p 3001:3001 product-catalog-microservice
   ```

3. Run other microservice or application containers on the same network:
   ```bash
   docker run --network my-network -p 4000:4000 ecommerce-ui
   ```

Docker-compose to set the environment variables, database and networking between all the micro-services

<img width="1994" height="625" alt="image" src="https://github.com/user-attachments/assets/38da58d8-3f03-4953-b035-87dcaed14c42" />

By connecting the containers to the same Docker network, they can communicate with each other using their container names as hostnames.

Create and account and login to application : localhost:4000

<img width="2544" height="791" alt="image" src="https://github.com/user-attachments/assets/c06192e2-2ef3-4000-8b91-5125654c6d2a" />

Here we can see all the micro-services

<img width="2317" height="1345" alt="image" src="https://github.com/user-attachments/assets/9d53ca68-20ef-4b0c-a75b-d856bfe9b4c9" />

CI pipeline, building the image and pushing the image to docker hub by using GitHub-actions 

Store the Docker Username and PAT in github

<img width="2127" height="1392" alt="image" src="https://github.com/user-attachments/assets/edb626de-6dc2-4646-a50e-e7a84108e95e" />

Make any changes to the code for specific micro service and pipeline will auto trigger and it will build that micro service only as we have used filters option

for eg: Here I have updated the ecommerce-ui microservice, pipeline will trigger and build that micro service only

<img width="1510" height="1402" alt="image" src="https://github.com/user-attachments/assets/28851028-4488-4735-a1ed-1e8db60d3d06" />

<img width="2557" height="771" alt="image" src="https://github.com/user-attachments/assets/81c736ef-3103-4b26-ae1c-d5248ee6965a" />

---

## Install Argo CD

```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

```

## Expose Argo CD server (for external access):

```bash

kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'

```

Wait and get the public IP:

```bash
kubectl get svc -n argocd
```

Get admin password:

```bash

kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d

```

Login to ArgoCD UI at http://<EXTERNAL-IP> with username admin and the password above.

## Configure Argo CD to Sync with Repo

In the Argo CD UI:

Click New App

Set:

App name: ecommerce-app
Project: `default`
Repo URL: your GitHub repo (HTTPS)
Path: `manifest`
Cluster URL: `https://kubernetes.default.svc`
Namespace: `ecommerce`
Enable Auto-sync

Now ArgoCD will deploy whatever’s in `manifest/` automatically.
