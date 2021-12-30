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


## Deploying a Sprinig Boot Application in Kubernetes

- First, we need to connect to the Kubernetes Cluster, GCP give us the Google Cloud Shell, a terminarl to connect to the cluster.

![image](https://user-images.githubusercontent.com/36638342/147717708-e75985d3-1852-45c1-a51f-81216c0b255e.png)

![image](https://user-images.githubusercontent.com/36638342/147717733-3d4c6075-5cb9-4b17-a086-231058b79919.png)

** You can decouple the Terminal to another tab

- Click on "Connect" to get the command to connect to the Kubernetes Cluster.

![image](https://user-images.githubusercontent.com/36638342/147717877-2c926dc3-1673-4282-8e42-7bfaffb7c7c8.png)

- Remember ``kubectl`` is the main command to interact with the Kubernetes Cluster.
- ``kubectl create deployment hello-world-rest-api --image=in28min/hello-world-rest-api:0.0.1.RELEASE`` in the terminal, this command deploys an app.
- ``kubectl expose deployment hello-world-rest-api --type=LoadBalancer --port=8080`` Expose to the world the image (App) created in the step above with Load Balancer and using the port 8080.

![image](https://user-images.githubusercontent.com/36638342/147719483-83607e32-6b01-4589-ab1f-7600730952cd.png)

- Go to "Services & Ingress" in GPC, the app should appear there.

![image](https://user-images.githubusercontent.com/36638342/147719525-2b8e98e0-fe3f-4349-88d6-a713de314713.png)

- If you see "OK" in the status you can click on the endopoint link to review if the instace is running.

![image](https://user-images.githubusercontent.com/36638342/147719631-e7bee131-d3f6-4d6e-b830-db22f615176e.png)



## Concepts
- K8S --> Kubernetes abreviation.
- AKS, EKS and GKE names of services in the cloud.
- ``kubectl`` is the main command to interact with the Kubernetes Cluster.
- Kubernetes follows the principle of "single responsability", this means each component only have onw task assigned and only one.


### PODS

![image](https://user-images.githubusercontent.com/36638342/147720273-fbc2df92-4c4b-49ac-a58f-7da8af065c17.png)

- Is the smallest deployable unit in Kubernetes.
- Yes, even smaller than a container.
- You can't have a container in Kubernetes without a Pod.
- The container LIVES INSIDE of a Pod.
- ``kubectl get pods``
  - List with all the Pods created 
- ``kubectl get pods -o wide``
  - List with all the Pods created including more details

![image](https://user-images.githubusercontent.com/36638342/147720437-5285281d-d25b-4e1f-8f01-c3cc19b9e552.png)

- Each Pod have an unique IP address.
- A Pod can contain one or more containers (READY Column)
  - You can see how many of them are and how many are ready.
- ``kubectl explain pods``
  - Explain the definition of what is a Pod.
- ``kubectl describe pod <id>``
  - Details about the Pod.
  - IP, node and NAMESPACE.
  - NAMESPACE is used to isolate Pods from another Pod.
  - Label, using Labels is the way to link Pods, ReplicaSets, Deployments and Services.
  - Annotations, metadata of the Pod.
  - Status, current status of the Pod.

![image](https://user-images.githubusercontent.com/36638342/147721018-437e89dd-4c72-49af-b06c-36be1890b924.png)





# Commands
``kubectl version`` --> to know the verison of Kubernetes running

``kubectl create deployment hello-world-rest-api --image=in28min/hello-world-rest-api:0.0.1.RELEASE`` --> this command deploys an app.
  - --image=repo/image:tag took from Docker Hub

``kubectl expose deployment hello-world-rest-api --type=LoadBalancer --port=8080`` 
  - Expose to the world the image (App) created with that name.
  - type indicates that it will have Load Balancer.
  - port indicates the port to expose it (8080).

``kubectl get events`` 
  - Show the list of events occurred 
  - Including the events to create a pod, a replicaSet and a deployment
  - 
![image](https://user-images.githubusercontent.com/36638342/147719836-2c4f7068-9623-46e2-9339-e269bf68abce.png)

``kubectl get pods``
  - Displays a list with all the pods created

![image](https://user-images.githubusercontent.com/36638342/147719958-d4938e9c-8ac9-4ec5-b101-38a7e2de1cb5.png)


``kubectl get pods -o wide``
  - Displays a list with all the pods created with extra details like IP, Nodes, etc.

![image](https://user-images.githubusercontent.com/36638342/147720433-10a76ce5-e0bb-4c1d-93e5-aa8c7f1ecabd.png)

``kubectl explain pods``
  - Explain the definition of what is a Pod.

``kubectl describe pod <id>``
  - Detaild about an specific Pod

![image](https://user-images.githubusercontent.com/36638342/147720845-0d87c0a5-45dc-4bcb-abaf-9c0866cc31e6.png)


``kubectl get replicaaset``
  - Displays a list with all the replicaSets created

``kubectl get deployment``
  - Displays a list with all the deployments created


``kubectl get service``
  - Displays a list of servies running

![image](https://user-images.githubusercontent.com/36638342/147720049-96077b19-6de6-47f1-9064-937285f0556d.png)



