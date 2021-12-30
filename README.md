# kubernetes-udemy-microservices
Kubernetes section of UDEMY course https://www.udemy.com/course/microservices-with-spring-boot-and-spring-cloud/


# Scenario

Lets imagine this scenario:

- 10 instances of Microservice 
- 15 instances of another Microservice 
- Auto-Scalling
- Auto-Discovery of new Microservices
- Load Balancer
- Health Checks and replacing of failing instances
- Zero Downtime deployments if a want to upgrade from V1 to V2, V3, V4....

![image](https://user-images.githubusercontent.com/36638342/147192751-f93c01c7-dbca-495d-a130-0afdf2d3812b.png)


Then we need to implement a Container Orchestration, there are different Orchestrators, the most common is AWS ECS (Elastic Container Service) and there is also a Serverless version called AWS Fargate, but one of the most used is Kubernetes, that is a Cloud Neutral option, that means that can be implemented in the any of the clouds (AWS, Azure or Google Cloud), all of them have a service dedicated to run Kubernetes:

- AWS --> Elastic Kubernetes Service (EKS)
- Azure --> Azure Kubernetes Service (AKS)
- GPC --> Google Kubernetes Engine (GKE)

![image](https://user-images.githubusercontent.com/36638342/147193741-41b789b4-adc5-43e9-b065-ebaa4a8affa1.png)

**The idea of use Google Cloud Platform (GPC) is because is the only one that offers a free tier, AWS ans Azure don't have it.


# GKE (Google Cloud Engine)
- It's a service of the Google Cloud Platform where you can configure a Kubernetes cluster.
- Kubernetes is Cloud Neutral, that means can run in any service of the cloud (Azure, AWS, GCP)

## Getting a Google Cloud Platform Account
- Go to https://cloud.google.com/

![image](https://user-images.githubusercontent.com/36638342/147715577-3e77ebdc-125f-4f2b-aa6d-2e6049f1408b.png)

- Click on "Get Started for Free"
- Fillout all the forms, this will give you $300 credit to spend in the next 12 months in GCP.


#### Kubernetes Cluster
It'a group of servers managed together. 

#### Virtual Servers in Cloud
- Kubernetes manages servers, virtual servers in the cloud.
- Depending of the Cloud service is the name of this servers.
  - Amazon calls them EC2 (Elastic Compute Cloud).
  - Azure calls them Virtual Machines.
  - GCP calls them Compute Engines.
  - Kubernetes calls them Nodes.


- If you have a lot of things to manage,  you need a "Manager", that will manage the cluster of Nodes (Master Nodes).
  - e.g. If you have a thousand of Nodes you need some Master Nodes to manage them
  - Typically you only have one Master Node, but when you need high availability you need multiple of them.


Then... What is a Cluster?
- A Cluster is a combination of Nodes and a Master Node.
- The Master Node is in charge to keep the Nodes working and available.

![image](https://user-images.githubusercontent.com/36638342/147716243-2542ac77-a9cd-41a3-9be3-74b3618fd9c7.png)




## Create a Kubernetes Cluster in GCP
- Go to GPC
- Search for "Kubernetes Engine" --> is the service of GPC for Kubernetes

![image](https://user-images.githubusercontent.com/36638342/147716337-410c2d19-61df-4a5a-8007-4962ee0fe94d.png)

- Enable it

![image](https://user-images.githubusercontent.com/36638342/147716381-0aaf572f-c6aa-4542-961c-4ebfd533582d.png)

![image](https://user-images.githubusercontent.com/36638342/147716563-4f20fbd8-a871-4965-8607-e14fffa582b3.png)

- Clusters
  - Contains information about the clusters created.
- Workload
  - To manage the applications in the containers running in the cluster.
- Services & Ingresses
  - To configure the access to your workloads (applications) from the external world.
- Configuration
  - To configure the applications.
- Storage
  - Configuration of the storage  to persiste the data for the applications.


- Click on "Create" button

![image](https://user-images.githubusercontent.com/36638342/147716830-4aa92417-2f53-4cec-905f-22c10e1ce315.png)

- You can configure the name of the cluster, zone, region, number of nodes, etc
  - You can left the default values and only change the name
- Click on "Create"
- The cluster will be created
  - Take in consideration that you can be charged $$ if you leave the cluster running, then if you are only using it for learning purposes, delete it always and create it every time you continue with your lessons.

![image](https://user-images.githubusercontent.com/36638342/147717149-28a960d8-44fb-4a00-ba69-0f9ebe966fec.png)


## Reviewing Kubernetes Cluster
- If you click on the Kubernetes Cluster you can see the details of it.

![image](https://user-images.githubusercontent.com/36638342/147717329-587f3d36-35f5-47ff-85f2-c8a3bcf66b24.png)

- If you click on "Nodes" you will seee the number of nodes created, the memory and the CPU assigned to each one of them

![image](https://user-images.githubusercontent.com/36638342/147717379-af8c13a6-36f2-4d98-9d59-99b350044395.png)

- If the information about memory and CPU of each node is different than the displayed in the main screen, is because Kubernetes takes part of the memory and CPU of each one of the nodes to him, this allows Kubernetes to manage the nodes and be able to execute commands.




## Concepts
- K8S --> Kubernetes abreviation.
- AKS, EKS and GKE names of services in the cloud.
- 
