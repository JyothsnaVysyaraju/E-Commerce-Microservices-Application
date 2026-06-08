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

Create an account and login to application : localhost:4000

<img width="2544" height="791" alt="image" src="https://github.com/user-attachments/assets/c06192e2-2ef3-4000-8b91-5125654c6d2a" />

Here we can see all the micro-services

<img width="2317" height="1345" alt="image" src="https://github.com/user-attachments/assets/9d53ca68-20ef-4b0c-a75b-d856bfe9b4c9" />

CI pipeline, building the image and pushing the image to docker hub by using GitHub-actions 

Store the Docker Username and PAT in github secrets 

<img width="2127" height="1392" alt="image" src="https://github.com/user-attachments/assets/edb626de-6dc2-4646-a50e-e7a84108e95e" />

Make any changes to the code for specific micro service and pipeline will auto trigger and it will build that micro service only as we have used filters option

for eg: Here I have updated the ecommerce-ui microservice, pipeline will trigger and build that micro service only

<img width="1510" height="1402" alt="image" src="https://github.com/user-attachments/assets/28851028-4488-4735-a1ed-1e8db60d3d06" />

<img width="1292" height="944" alt="image" src="https://github.com/user-attachments/assets/ca029bfa-f19c-4fc8-aeb3-ef0d52d5cf1c" />

<img width="2557" height="771" alt="image" src="https://github.com/user-attachments/assets/81c736ef-3103-4b26-ae1c-d5248ee6965a" />

**For CD we will setup EKS cluster using shell scripting**

**Provide the below IAM access to insatll the EKS cluster** 

<img width="2051" height="974" alt="image" src="https://github.com/user-attachments/assets/6811bf24-d25a-4d8a-9a16-c97f6bfe726e" />

<img width="1979" height="1258" alt="image" src="https://github.com/user-attachments/assets/4d8090e3-8a39-4b87-a9b6-c22e146e22c8" />

<img width="1988" height="1130" alt="image" src="https://github.com/user-attachments/assets/6fb07a77-ee92-44a8-94ba-61d8392c99f6" />

<img width="1494" height="405" alt="image" src="https://github.com/user-attachments/assets/87849840-f1a0-4cce-9cca-1a121454db24" />

**Cluster and nodes got created**
**Now we have to install alb-controller**

<img width="1969" height="1292" alt="image" src="https://github.com/user-attachments/assets/9b134702-19d1-45af-b7d6-79afda5c67c4" />

<img width="1100" height="381" alt="image" src="https://github.com/user-attachments/assets/9b7b5813-a8f7-45f2-a997-b5d4d64f2832" />

**install CSI driver**

<img width="1979" height="1259" alt="image" src="https://github.com/user-attachments/assets/fdbf11fc-316a-4f21-a827-a1347a8312e0" />

helm upgrade --install aws-ebs-csi-driver \
  --namespace kube-system \
  --set controller.serviceAccount.create=false \
  --set controller.serviceAccount.name=ebs-csi-controller-sa \
  aws-ebs-csi-driver/aws-ebs-csi-driver \
  --timeout 10m

<img width="1992" height="963" alt="image" src="https://github.com/user-attachments/assets/a1430ca5-c6b3-477c-b289-732e33ad253d" />

<img width="1986" height="507" alt="image" src="https://github.com/user-attachments/assets/46ac33cc-5e95-4041-8ad2-51653420693e" />

✅ EKS cluster is created
✅ IAM role for EBS CSI exists
✅ ServiceAccount is bound correctly
✅ EBS CSI driver is deployed and healthy
✅ EBS volumes can be dynamically provisioned

**cluster is up and running , we have ALB and CSI driver installed on cluster**

<img width="2549" height="458" alt="image" src="https://github.com/user-attachments/assets/abe0c8a8-ad5f-49f0-8230-2750f27ebcf8" />

<img width="2555" height="877" alt="image" src="https://github.com/user-attachments/assets/6b30505e-7b69-47b6-bbf1-f5491466c759" />

<img width="1654" height="1009" alt="image" src="https://github.com/user-attachments/assets/f047f27a-26cf-4131-a1cb-66fea74fac16" />

<img width="2556" height="869" alt="image" src="https://github.com/user-attachments/assets/e2295d4a-41dd-431b-8003-22f2cb1ba24b" />

<img width="2560" height="1165" alt="image" src="https://github.com/user-attachments/assets/c13e1ed5-301f-4874-8a12-52da802c8310" />


The EBS CSI driver creates EBS volumes in EC2, not in the EKS console directly.
The EKS console shows the addon and configuration, but not the actual volumes.
To see the real volumes, you always go to EC2 → Volumes.

<img width="1128" height="940" alt="image" src="https://github.com/user-attachments/assets/53e04735-13cb-4271-bfc7-477521ced5e5" />

**setup the ArgoCD for GitOps to implement Continuous Deployment**

---

## Install Argo CD

```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

```
<img width="1722" height="568" alt="image" src="https://github.com/user-attachments/assets/7907391f-0ddd-472c-8345-3cfab6f1b6e6" />

All pods are Running with READY:
argocd-application-controller-0 → 1/1 Running
argocd-applicationset-controller-6d9bc95cc7-f6wzh → 1/1 Running
argocd-dex-server-59d47894bd-5p2kn → 1/1 Running
argocd-notifications-controller-ddb4b696f-2ttpz → 1/1 Running
argocd-redis-5986f9f49b-kxfp6 → 1/1 Running
argocd-repo-server-54d9c5f7cb-bvzc5 → 1/1 Running
argocd-server-74c7bdd997-kjfl9 → 1/1 Running
All services are also created, including:
argocd-server (ports 80/TCP, 443/TCP)
argocd-repo-server
argocd-applicationset-controller
argocd-redis
argocd-metrics
This confirms:
✅ Your EKS cluster dev-eks is complete ✅ Node group standard-workers is active (3 nodes) ✅ EBS CSI driver is running (EBS volumes can be provisioned) ✅ VPC CNI, CoreDNS, kube-proxy, Metrics Server addons are active ✅ Argo CD is deployed and healthy in the argocd namespace

Now we have to change the argocd from cluster ip as service to load balancer so that we can access it using load balancer

## Expose Argo CD server (for external access):

```bash

kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'

```
<img width="1962" height="623" alt="image" src="https://github.com/user-attachments/assets/be7d952f-b988-4065-85fc-eda4b297bdab" />

we can access the argocd using loadblancer service

get the argocd credentials:                        

```bash

kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d

```
## Login to ArgoCD UI at http://<EXTERNAL-IP> with username admin and the password above.

<img width="2533" height="1031" alt="image" src="https://github.com/user-attachments/assets/4e06dc71-bd09-4336-9a18-1765b73a395b" />

## Configure Argo CD to Sync with Repo

In the Argo CD UI:

Click New App

Set:

<img width="1747" height="1407" alt="image" src="https://github.com/user-attachments/assets/ee580a6a-4212-4ffa-8b03-ca7fadd6a96d" />

create the application

<img width="1888" height="1466" alt="image" src="https://github.com/user-attachments/assets/65ae5d28-245c-4aeb-932f-8287e09dc379" />

<img width="1691" height="1346" alt="image" src="https://github.com/user-attachments/assets/2e7e5bf3-004a-4aad-840e-028ef19ace7b" />

<img width="2331" height="1410" alt="image" src="https://github.com/user-attachments/assets/42ab65d5-5385-4f2b-978b-8bcbbd3f719f" />


Now ArgoCD will deploy whatever’s in `manifest/` automatically.

<img width="1876" height="801" alt="image" src="https://github.com/user-attachments/assets/5826ded5-e0f4-4506-89f9-9eb9550616ab" />

<img width="2341" height="1281" alt="image" src="https://github.com/user-attachments/assets/f08948b2-28f0-44ff-9757-e450c84067b8" />

<img width="2342" height="1392" alt="image" src="https://github.com/user-attachments/assets/c9b5cafa-0f69-4537-ad47-ad43ced46339" />

<img width="2467" height="1000" alt="image" src="https://github.com/user-attachments/assets/31182c81-83ac-4214-9201-efe13880ec27" />




