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


# Kubernetes Architecture

![image](https://user-images.githubusercontent.com/36638342/147837585-6da6984e-c6ed-4154-9cef-428e26c884a6.png)

- Master Nodes
- Worker Nodes
- Cluster

## Master Node
- It has a lot af components, is the node  in charge to manage the rest of nodes in the cluster.
- The Master Node the applications doesn't have an application running inside, that's part of the work of the pods inside of the Worker Nodes.

![image](https://user-images.githubusercontent.com/36638342/147837616-4d07768d-d35e-47e2-b804-7ab504d7621d.png)

### Distributed Database (ETCD)
- Running as part of the Master Node.
- Configurations, Scallin operations, deployments are stored here.
  - Is called "Desired State" when we send a command to Kubernetes.


### API-SERVER (kube-apiserver)
- API used by GCP to talk with Kubernetes.
- When we send a command using the console, the command is submitted to that API.

### Scheduler (kube-scheduler)
- Schedule the pods on to the nodes.
- Decides which node will take the new pod, based on memory, CPU, etc.

### Controller Manager (kube-controller-manager)
- Manages the overall health.
- When we want a lot of instances of a different applications, the Controller Manager manages the way is deployed to keep the cluster healthy.


If the Master Node is down, the applications related to it are down also?
No, even if the Master Node is down, the applications keeps running because they are in the Worker Nodes, maybe you can't do any change on them, but they will be still alive.

If we run ``kubectl get componentstatuses`` we will get the status of Master Node components.

![image](https://user-images.githubusercontent.com/36638342/147839061-cfac87b8-fa15-4c17-af2d-29f6fe7af72f.png)


## Worker Nodes
- They are in charge to run the applications in the pods.

![image](https://user-images.githubusercontent.com/36638342/147838340-5e91925f-7cd3-4000-bfdb-58cbfa5b5606.png)

### Node Agent (kubelet)
- Monitoring what happens on the Node and communicates it to the Master Node.
- e.g. if a Pod gows down, Node Agent reports to the Controller Manager in the Master Node.

### Networking Component (kube-proxy)
- Expose Services around the Nodes and the Pods.

### Container Runtime (CRI - Docker, rkt, etc)
- We want to run containers inside of the Pods, that happens because the Container Runtime.
  - This means that we can run different type of containers compatible with OCI not only Docker containers. 
  - The most frequently used is Docker.

### PODS
- A Node can have a lot of pods and inside of the pods could be more than of applications running.
- The container LIVES INSIDE of a Pod, a Pod can have multiple containers inside.



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
- Kubernetes follows the principle of "single responsability", this means each component only have one task assigned and only one.


### PODS

![image](https://user-images.githubusercontent.com/36638342/147720273-fbc2df92-4c4b-49ac-a58f-7da8af065c17.png)

- Is the smallest deployable unit in Kubernetes.
- Yes, even smaller than a container.
- You can't have a container in Kubernetes without a Pod.
- The container LIVES INSIDE of a Pod, a Pod can have multiple containers inside.
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


### REPLICASETS

- Ensures that a specific number of Pods are running at all times.
- ``kubectl get replicasets`` 
- ``kubectl get replicaset`` 
- ``kubectl get rs``

![image](https://user-images.githubusercontent.com/36638342/147721544-a96c081d-3f82-4773-a312-adc1ea4635ae.png)

- DESIRED, number of pod desired.
- CURRENT, How many of them are running.
- READY, How many of them are ready.
-``kubectl get rs -o wide``
  - More dedails of the replicaSet

![image](https://user-images.githubusercontent.com/36638342/147721627-a368b4d2-6706-4932-b454-4f5529d1f83a.png)

- If we do ``kubectl delete <ID of Pod>``
  - Delete the specific Pod
- And then ``kubectl get pod -o wide`` to display the current Pods
- We will see a Pod running with a new ID

![image](https://user-images.githubusercontent.com/36638342/147721859-28fbf9b5-edaa-4a26-b0b6-621dcc86635b.png)

- This is because the ReplicaSet is in charge to keep the Pods running, and as we can see above, it's configured to have 1 DESIRED, then, when the number becomes 0, the ReplicaSet creates a new one.

- ``kubectl scale deployment hello-world-rest-api --replicas=3``
  - Configures the number of replicas of the Pod to minimum three 

![image](https://user-images.githubusercontent.com/36638342/147722178-4ecd5916-709b-49b8-8201-33f9d37966d2.png)


### DEPLOYMENTS

- It's used when want Zero Downtime upgrading an application, it means, pass from V1 to V2 of an application.
- ``kubectl get rs -o wide`` to get info about the ReplicaSets.

![image](https://user-images.githubusercontent.com/36638342/147722747-94b5d289-9b66-4c0a-bd2c-c7e3c78b6f09.png)

- We will see the version or the image attached to that ReplicaSet.

Let's asume that we want to deploy a new version of "hello-world-rest-api"
- If we run ``kubectl set image deployment hello-world-rest-api hello-world-rest-api=DUMMY_IMAGE:TEST``

![image](https://user-images.githubusercontent.com/36638342/147723340-0deb3898-2991-4a6d-921b-bf5c3ce91551.png)

- This will try to replace the "hello-world-rest-api" app with this version "hello-world-rest-api=DUMMY_IMAGE:TEST" (Same app, but with the image DUMMY_IMAGE and Tag TEST"
- We know that image and tag doesn't exist, but the application is still alive because the ReplicaSets with the old version.
- If we executes ``kubectl get rs -o wide`` we will see one pod down and two alive, the one down tried to deploy the new version, the another two pods are still running the old version.

![image](https://user-images.githubusercontent.com/36638342/147723516-23ba053f-7cd5-4c6c-804f-5231def29c2e.png)

- If we run ``kubectl get pods`` we will see three instances running and a new one that fails

![image](https://user-images.githubusercontent.com/36638342/147723643-684feffe-dfee-4276-9a6e-d6b82cfd6ed3.png)

Let's do it with a vlaid image

- Run ``kubectl set image deployment hello-world-rest-api hello-world-rest-api=in28min/hello-world-rest-api:0.0.2.RELEASE`` (Repo, image and tag took from DockerHub)
- Run ``kubectl get pods`` and you can see how the Deployment is replacing the pods using the replicaSets.

![image](https://user-images.githubusercontent.com/36638342/147723927-dc34252d-3c37-4fb9-bc28-1c40a30a765a.png)

![image](https://user-images.githubusercontent.com/36638342/147724165-06c182f5-4432-4ce2-b42c-c26b9d0fda28.png)



### SERVICES

- Run ``kubectl get pods -o wide`` you can see that each pod have it's own IP.
- Kill one pod with ``kubectl delete pod <ID>``
- Run again``kubectl get pods -o wide`` you will see a new pod running with a new Id and a new IP.

![image](https://user-images.githubusercontent.com/36638342/147724519-8000356d-7cfc-433c-9cd1-875f33c3c66c.png)

- In the "Front" of the applications, we are still using the same url in the browser to access to the app, and behind that the Load Balancer is sending the requests to the three different pods, it doesn't matter if we deleted any of them and the IP changed, Kubernetes find the pod and balance the requests again.
- That "Magic" because a SERVICE is running in the background
  - His role is provide an always-available external interface to the applications which are running inside the pods.
- The service was created when, at the beginning, we executed ``kubectl expose deployment hello-world-rest-api --type=LoadBalancer --port=8080`` 
  - GCP created a Load Balancer service for us.
- If you go in GCP and search for Load Balancing, you can find the application running and see the IP in the frontend and the backend instances.

![image](https://user-images.githubusercontent.com/36638342/147724913-ac94339e-8535-4582-8aac-06df4462b703.png)

- Run ``kubectl get services`` and you will see the Load Balancer service running

![image](https://user-images.githubusercontent.com/36638342/147725368-9f6c4cd1-a175-4792-8263-3ec8c790155b.png)




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

![image](https://user-images.githubusercontent.com/36638342/147719836-2c4f7068-9623-46e2-9339-e269bf68abce.png)

``kubectl get events --sort-by=.metadata.creationTimestamp``
  - Filter the data in the events command

![image](https://user-images.githubusercontent.com/36638342/147722312-e33ad6da-49cd-4c67-9351-758db0237827.png)



#### PODS
``kubectl get pods``
  - Displays a list with all the pods created

![image](https://user-images.githubusercontent.com/36638342/147719958-d4938e9c-8ac9-4ec5-b101-38a7e2de1cb5.png)


``kubectl get pods -o wide``
  - Displays a list with all the pods created with extra details like IP, Nodes, etc.

![image](https://user-images.githubusercontent.com/36638342/147720433-10a76ce5-e0bb-4c1d-93e5-aa8c7f1ecabd.png)

``kubectl explain pods``
  - Explain the definition of what is a Pod.

``kubectl explain replicaset``
  - Explain the definition of what is a ReplicaSet.

``kubectl describe pod <id>``
  - Detaild about an specific Pod

![image](https://user-images.githubusercontent.com/36638342/147720845-0d87c0a5-45dc-4bcb-abaf-9c0866cc31e6.png)

``kubectl delete <ID of Pod>``



#### REPLICASET

``kubectl get replicaset``
  - Displays a list with all the replicaSets created

``kubectl get replicaset -o wide``
  - More dedails of the replicaSet

![image](https://user-images.githubusercontent.com/36638342/147721627-a368b4d2-6706-4932-b454-4f5529d1f83a.png)


``kubectl scale deployment hello-world-rest-api --replicas=3``
  - Configures the number of replicas of the Pod to minimum three


#### DEPLOYMENT

``kubectl get deployment``
  - Displays a list with all the deployments created

``kubectl set image deployment hello-world-rest-api hello-world-rest-api=DUMMY_IMAGE:TEST``
  - Replaces the deployment with another image.
  - It follows the format ``kubectl set image deployment <current_app> <new_app>=<IMAGE>:<Tag>``
  - If there are more than one pod running the container, Kubernetes will try to update one pod and if it's not possible, Kubernetes will keep the rest pf pods with the old version to have Zero downtime.

![image](https://user-images.githubusercontent.com/36638342/147723573-97d4c7e7-e0a5-4ecc-955f-ecaad0ee864b.png)


![image](https://user-images.githubusercontent.com/36638342/147723619-12f3619b-7f91-41ee-a474-c1117b32fa53.png)



``kubectl get service``
  - Displays a list of servies running

![image](https://user-images.githubusercontent.com/36638342/147720049-96077b19-6de6-47f1-9064-937285f0556d.png)

![image](https://user-images.githubusercontent.com/36638342/147725386-87e0aefd-be87-4fbd-9d73-11e35d813d4c.png)


``kubectl get componentstatuses``
- Get status of the components of the Master Node 

![image](https://user-images.githubusercontent.com/36638342/147839050-87197dff-4b1b-4235-95b1-6d8cc14c7d8a.png)



# Installing GCloud and Kubectl
- We can install the Google Cloud CLI to use in our machine instead of use the one embeded in the Google Cloud Platform
- Kubectl is the CLI to use the ``kubectl`` commands for Kubernetes.

## Gcloud (Google Cloud SDK)
- Search in the browser "Google Cloud SDK".
- Go to the website and install it (For Windows there is an installer).
- https://cloud.google.com/sdk/docs/quickstart
- The installation will ask to login, authotize the use of your google account and select the project you want to use, the project is the same we have in the GCP bash
  - You can do it also using the command ``gcloud init``

![image](https://user-images.githubusercontent.com/36638342/147839312-7747b5f3-2e89-42d3-b915-14c654c9d516.png)


![image](https://user-images.githubusercontent.com/36638342/147839253-a1b31e26-a2ec-46fd-8af9-d10e2cff0748.png)


- ``gcloud auth login``  will send you again to the login page.

## Installing Kubectl
- Go to https://kubernetes.io/es/docs/tasks/tools/install-kubectl/
- Follow the instructions depending you OS ( lol )
- For Windows there is an installer.

![image](https://user-images.githubusercontent.com/36638342/147839398-3f4ffa7a-88b0-49e4-8f0f-f2094a8851bd.png)

You can try ``kubectl version``to know the version installed

Now you can go to GCP, copy the command to connect to you Kubernetes cluster in GCP and execute in your local terminal.

![image](https://user-images.githubusercontent.com/36638342/147839420-32f7596e-3e31-4134-a1e8-3c49cb35b073.png)

![image](https://user-images.githubusercontent.com/36638342/147839430-bd6847e6-65e2-40b6-a2f7-3aac699c5366.png)



