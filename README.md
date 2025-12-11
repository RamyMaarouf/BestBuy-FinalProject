# Best Buy Microservices Application
This project implements a scalable e-commerce application inspired by the Best Buy retail environment, deployed on Azure Kubernetes Service (AKS). 
The architecture employs a microservices pattern with asynchronous messaging to ensure fast checkout and resilient order fulfillment.


## 1. Application Explanation

**Brief Application Explanation**

This application simulates the core purchasing journey of a large retailer. It allows customers to browse a product catalog, manage a shopping cart, and submit orders for electronic items. 
The system is engineered around service independence, where each business capability is managed by a separate service.

**Design Choices**

Kubernetes (AKS): Chosen for its robust capabilities in managing containerized workloads at scale. 
Using AKS provides auto-scaling to handle peak shopping events and guarantees self-healing if any service fails.

Asynchronous Messaging (RabbitMQ): The order-service delegates the complex task of shipping processing to the makeline-service via a RabbitMQ queue. 
This design ensures the customer's transaction is completed instantly, while fulfillment occurs reliably in the background, crucial for high-volume retail.

API Gateway Pattern: The store-front container (Vue.js + Nginx) acts as the single point of entry. 
By routing simple relative calls internally to product-service.



## 2. Architecture Diagram

The system architecture is divided into front-facing, internal logic, and persistent infrastructure components:

1. store-front: Customer UI, Nginx Proxy. The entry point exposed via LoadBalancer.
2. product-service: Manages the retail product catalog, prices, and inventory.
3. order-service: Handles all customer order submissions, persists data, and publishes order events.
4. makeline-service: Simulates the warehouse/fulfillment logic by processing orders consumed from RabbitMQ.
5. mongodb: Persistent storage for all application data (Products, Orders).
6. rabbitmq: Message broker for guaranteed asynchronous communication.

## 3. Deployment Instructions

This section outlines the steps necessary to launch the entire microservices application on an existing AKS cluster.

**Prerequisites**

The following tools were installed and configured: git, docker, kubectl, and the Azure CLI (az).
1. Set Azure Context: Ensure kubectl is pointed at your AKS cluster: az aks get-credentials --resource-group [RESOURCE_GROUP] --name [AKS_CLUSTER_NAME] --overwrite-existing
2. Clone the Repos
3. Apply the unified YAML manifest. This will create all necessary Services, Deployments, the RabbitMQ StatefulSet, the MongoDB StatefulSet, and the configuration Secrets/ConfigMaps: kubectl apply -f aps-all-in-one.yaml
4. Wait until all pods show 1/1 under the READY column and Running status. This may take a moment as images are pulled and services initialize: kubectl get pods
5. The primary services are exposed using a Kubernetes Service of type LoadBalancer.
6. Get the Service details to find the external access point: kubectl get services store-front
7. Accessing the Store-Admin (Management App): kubectl get services store-admin

## 4. Links Table


## 🔗 Service Links and Image Registry

| Service Name | Code Repository Link | Docker Hub Image |
| :--- | :--- | :--- |
| **Store-Front** | [https://github.com/RamyMaarouf/store-front-finalproject1.git] | `maar0006/store-front-finalproject1:latest` |
| **Store-Admin** | [(https://github.com/RamyMaarouf/store-admin-finalproject1.git)] | `maar0006/store-admin-finalproject1:latest` |
| **Order Service** | [https://github.com/RamyMaarouf/order-service-finalproject1.git] | `maar0006/order-service-finalproject1:latest` |
| **Product Service** | [https://github.com/RamyMaarouf/product-service-finalproject1.git] | `maar0006/product-service-finalproject1:latest` |
| **Makeline Service** | [https://github.com/RamyMaarouf/makeline-service-finalproject1.git] | `maar0006/makeline-service-finalproject1:latest` |


