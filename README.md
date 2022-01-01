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

``kubectl get deployment currency-exchange -o yaml``
- Get YAML file of an application 

``kubectl get service currency-exchange -o yaml ``
- Get YAML file of service specified 

``kubectl diff -f deployment.yaml``
- Compares the YAML files with the current one deployed and displays it.

``kubectl apply -f deployment.yaml``
- Apply a YAML deployment file

``kubectl delete all -l app=currency-exchange``
- Delete everything related to the specified app


``kubectl create configmap currency-conversion --from-literal=CURRENCY_EXCHANGE_URI=http://currency-conversion``
- Creates a configmap mapping the value with the environment variable

``kubectl get configmap``
- Display the list of maps created



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



# Deploying Currency-Exchange and Currency-Conversion services in Kubernetes
- Go to https://github.com/in28minutes/spring-microservices-v2 and download the whole repo in zip.
- Extract only the "05. Kubernetes folder", inside are the two microservices mentioned in the title
- Changes made it in the microservices:
  - In the pom.xml
    - Changed Name and version of the SNAPSHOT
    - Commented AMQP, ZIPKIN, Spring Cloud Starter (To Maps) and Spring Cloud Eureka (To discovery) dependencies, Kubernetes has it's own versions of them.
  - In the Controllers
    - For the CurrencyConversionController
      - Added a Logger in the CurrencyConversionController
    - For the CurrencyExchangeController
      - Added a variable to the host using a property called HOSTNAME from the Environment Bean.
      - Added a variable to the version using a property called HOSTNAME from the Environment Bean.  
  - In the .properties file
    - Explicity enabled the Health indicators (Coming from the Actuator dependency)
  - For the CurrencyExchangePRoxy
    - Added a harcoded URL with a FeignClient to use Kubernetes discovery service instead of use EUREKA.
    - Uses the CURRENCY_ECHANGE_SERVICE_HOST environment variable or localhost if not exists.
    - This variable is automatically created by Kubernetes when a new pod is created.
      - SERVICE_NAME_SERVICE_HOST 



![image](https://user-images.githubusercontent.com/36638342/147840039-48670b19-07fa-44bb-a374-b663eabd1cb7.png)

- Replace in the pom.xml the name of your Docker Hub id.
- Execute Maven goals to create the images  (Remember you need Docker running in your machine)
  - ``spring-boot:build-image -DskipTests``
  - Do it for both microservices
- In your terminal execute ``docker login`` to login using the credentials you already have

![image](https://user-images.githubusercontent.com/36638342/147840418-4c878a40-36d8-4089-8211-6288a9de0965.png)

- Push the Currency Exchange image created with ``docker push hugotb88/mmv2-currency-exchange-service:0.0.11-SNAPSHOT"
  - User your own path created during the Maven build-image goal 

![image](https://user-images.githubusercontent.com/36638342/147840454-6e708151-0e69-44be-a3fd-b14f1dc5a60d.png)

- After push you can verify it in https://hub.docker.com/u/hugotb88

![image](https://user-images.githubusercontent.com/36638342/147840511-6d30fe12-1f1a-4f93-bae6-3915dc0be63d.png)

- Push now the Currency Conversion executingg ``docker push hugotb88/mmv2-currency-conversion-service:0.0.11-SNAPSHOT``

![image](https://user-images.githubusercontent.com/36638342/147840545-4690fe84-3e68-41c9-847a-762ca4591d95.png)


- In your terminal, once you are connected to you GCP instance.
- Execute ``kubectl create deployment currency-exchange --image=hugotb88/mmv2-currency-exchange-service:0.0.11-SNAPSHOT``
- Then ``kubectl expose deployment currency-exchange --type=LoadBalancer --port=8000`` to expose it
- You can verify using ``kubectl get services``

![image](https://user-images.githubusercontent.com/36638342/147840678-6ae64b79-9b84-4eb8-baa1-27de7e26a22e.png)

- You have the external IP, then you can perform a curl http://34.134.106.231:8000/currency-exchange/from/USD/to/INR to see if you get a response
- You can also do it from the browser.

![image](https://user-images.githubusercontent.com/36638342/147840752-c3f57d5c-37d2-4e4f-b7df-b71e3ffe9974.png)


# Create declarative YAML in Kubernetes
- The name of the file is ``deployment.yaml``
- Go to the project folder in your terminal
- Execute ``kubectl get deployment currency-exchange -o yaml``, it shows the YAML descriptive file in the terminal
- To save it ``kubectl get deployment currency-exchange -o yaml >> deployment.yaml``
- Execute the same for the service and  save it ``kubectl get service currency-exchange -o yaml >> service.yaml``

![image](https://user-images.githubusercontent.com/36638342/147840870-c3aec591-90e0-4189-add7-1e95eef85b22.png)

![image](https://user-images.githubusercontent.com/36638342/147840881-dcd66dee-889f-4dd0-ae99-a78ba257446d.png)

deployment.yaml

```
apiVersion: apps/v1
kind: Deployment
metadata:
  annotations:
    deployment.kubernetes.io/revision: "1"
  creationTimestamp: "2021-12-31T22:52:03Z"
  generation: 1
  labels:
    app: currency-exchange
  name: currency-exchange
  namespace: default
  resourceVersion: "73965"
  uid: 0fd62eba-df70-4df8-bb15-cf4cbf8f7f12
spec:
  progressDeadlineSeconds: 600
  replicas: 1
  revisionHistoryLimit: 10
  selector:
    matchLabels:
      app: currency-exchange
  strategy:
    rollingUpdate:
      maxSurge: 25%
      maxUnavailable: 25%
    type: RollingUpdate
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: currency-exchange
    spec:
      containers:
      - image: hugotb88/mmv2-currency-exchange-service:0.0.11-SNAPSHOT
        imagePullPolicy: IfNotPresent
        name: mmv2-currency-exchange-service
        resources: {}
        terminationMessagePath: /dev/termination-log
        terminationMessagePolicy: File
      dnsPolicy: ClusterFirst
      restartPolicy: Always
      schedulerName: default-scheduler
      securityContext: {}
      terminationGracePeriodSeconds: 30
status:
  availableReplicas: 1
  conditions:
  - lastTransitionTime: "2021-12-31T22:52:18Z"
    lastUpdateTime: "2021-12-31T22:52:18Z"
    message: Deployment has minimum availability.
    reason: MinimumReplicasAvailable
    status: "True"
    type: Available
  - lastTransitionTime: "2021-12-31T22:52:03Z"
    lastUpdateTime: "2021-12-31T22:52:18Z"
    message: ReplicaSet "currency-exchange-6bd8668498" has successfully progressed.
    reason: NewReplicaSetAvailable
    status: "True"
    type: Progressing
  observedGeneration: 1
  readyReplicas: 1
  replicas: 1
  updatedReplicas: 1

```

service.yaml

- Now, you have each one of the deployment.yaml files in their respective folders, but the idea is having a single deployment.yaml file. (Keep it in the Currency Exchange folder)
- You can cut and paste on into the another one separated by "---"


``deployment-01-initial.yaml``

```
apiVersion: apps/v1
kind: Deployment
metadata:
  annotations:
    deployment.kubernetes.io/revision: "1"
    kubectl.kubernetes.io/last-applied-configuration: |
      {"apiVersion":"apps/v1","kind":"Deployment","metadata":{"annotations":{"deployment.kubernetes.io/revision":"1"},"creationTimestamp":"2021-12-31T22:52:03Z","generation":3,"labels":{"app":"currency-exchange"},"name":"currency-exchange","namespace":"default","resourceVersion":"87625","uid":"0fd62eba-df70-4df8-bb15-cf4cbf8f7f12"},"spec":{"progressDeadlineSeconds":600,"replicas":1,"revisionHistoryLimit":10,"selector":{"matchLabels":{"app":"currency-exchange"}},"strategy":{"rollingUpdate":{"maxSurge":"25%","maxUnavailable":"25%"},"type":"RollingUpdate"},"template":{"metadata":{"creationTimestamp":null,"labels":{"app":"currency-exchange"}},"spec":{"containers":[{"image":"hugotb88/mmv2-currency-exchange-service:0.0.11-SNAPSHOT","imagePullPolicy":"IfNotPresent","name":"mmv2-currency-exchange-service","resources":{},"terminationMessagePath":"/dev/termination-log","terminationMessagePolicy":"File"}],"dnsPolicy":"ClusterFirst","restartPolicy":"Always","schedulerName":"default-scheduler","securityContext":{},"terminationGracePeriodSeconds":30}}},"status":{"availableReplicas":1,"conditions":[{"lastTransitionTime":"2021-12-31T22:52:18Z","lastUpdateTime":"2021-12-31T22:52:18Z","message":"Deployment has minimum availability.","reason":"MinimumReplicasAvailable","status":"True","type":"Available"},{"lastTransitionTime":"2021-12-31T22:52:03Z","lastUpdateTime":"2021-12-31T22:52:18Z","message":"ReplicaSet \"currency-exchange-6bd8668498\" has successfully progressed.","reason":"NewReplicaSetAvailable","status":"True","type":"Progressing"}],"observedGeneration":3,"readyReplicas":1,"replicas":2,"updatedReplicas":1}}
  creationTimestamp: "2021-12-31T22:52:03Z"
  generation: 4
  labels:
    app: currency-exchange
  name: currency-exchange
  namespace: default
  resourceVersion: "89248"
  uid: 0fd62eba-df70-4df8-bb15-cf4cbf8f7f12
spec:
  progressDeadlineSeconds: 600
  replicas: 1
  revisionHistoryLimit: 10
  selector:
    matchLabels:
      app: currency-exchange
  strategy:
    rollingUpdate:
      maxSurge: 25%
      maxUnavailable: 25%
    type: RollingUpdate
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: currency-exchange
    spec:
      containers:
      - image: hugotb88/mmv2-currency-exchange-service:0.0.11-SNAPSHOT
        imagePullPolicy: IfNotPresent
        name: mmv2-currency-exchange-service
        resources: {}
        terminationMessagePath: /dev/termination-log
        terminationMessagePolicy: File
      dnsPolicy: ClusterFirst
      restartPolicy: Always
      schedulerName: default-scheduler
      securityContext: {}
      terminationGracePeriodSeconds: 30
status:
  availableReplicas: 1
  conditions:
  - lastTransitionTime: "2021-12-31T22:52:18Z"
    lastUpdateTime: "2021-12-31T22:52:18Z"
    message: Deployment has minimum availability.
    reason: MinimumReplicasAvailable
    status: "True"
    type: Available
  - lastTransitionTime: "2021-12-31T22:52:03Z"
    lastUpdateTime: "2021-12-31T22:52:18Z"
    message: ReplicaSet "currency-exchange-6bd8668498" has successfully progressed.
    reason: NewReplicaSetAvailable
    status: "True"
    type: Progressing
  observedGeneration: 4
  readyReplicas: 1
  replicas: 1
  updatedReplicas: 1

---

apiVersion: v1
kind: Service
metadata:
  annotations:
    cloud.google.com/neg: '{"ingress":true}'
    kubectl.kubernetes.io/last-applied-configuration: |
      {"apiVersion":"v1","kind":"Service","metadata":{"annotations":{"cloud.google.com/neg":"{\"ingress\":true}"},"creationTimestamp":"2021-12-31T22:52:55Z","finalizers":["service.kubernetes.io/load-balancer-cleanup"],"labels":{"app":"currency-exchange"},"name":"currency-exchange","namespace":"default","resourceVersion":"87629","uid":"9c0c1f68-7e3f-4f05-aef1-14c1ee315dab"},"spec":{"clusterIP":"10.52.1.21","clusterIPs":["10.52.1.21"],"externalTrafficPolicy":"Cluster","ipFamilies":["IPv4"],"ipFamilyPolicy":"SingleStack","ports":[{"nodePort":32164,"port":8000,"protocol":"TCP","targetPort":8000}],"selector":{"app":"currency-exchange"},"sessionAffinity":"None","type":"LoadBalancer"},"status":{"loadBalancer":{"ingress":[{"ip":"34.134.106.231"}]}}}
  creationTimestamp: "2021-12-31T22:52:55Z"
  finalizers:
    - service.kubernetes.io/load-balancer-cleanup
  labels:
    app: currency-exchange
  name: currency-exchange
  namespace: default
  resourceVersion: "89253"
  uid: 9c0c1f68-7e3f-4f05-aef1-14c1ee315dab
spec:
  clusterIP: 10.52.1.21
  clusterIPs:
    - 10.52.1.21
  externalTrafficPolicy: Cluster
  ipFamilies:
    - IPv4
  ipFamilyPolicy: SingleStack
  ports:
    - nodePort: 32164
      port: 8000
      protocol: TCP
      targetPort: 8000
  selector:
    app: currency-exchange
  sessionAffinity: None
  type: LoadBalancer
status:
  loadBalancer:
    ingress:
      - ip: 34.134.106.231

```

- Try changing the "replicas" (The first one) parameter in Currency Exchange yaml to 2 and then execute ``kubectl apply -f deployment.yaml``

![image](https://user-images.githubusercontent.com/36638342/147840936-578b4f49-1093-4abf-8543-5739234e3798.png)

- You can execute ``kubectl get pods`` to verify that now you have two instances of currency-exchange running


### Cleaning up a declarative YAML
- You can remove some fields from a YAML file to keep it clean.

``deployment-02-cleaned-up.yaml``

```
apiVersion: apps/v1
kind: Deployment
metadata:
  annotations:
    deployment.kubernetes.io/revision: "1"
  labels:
    app: currency-exchange
  name: currency-exchange
  namespace: default
spec:
  replicas: 1
  selector:
    matchLabels:
      app: currency-exchange
  strategy:
    rollingUpdate:
      maxSurge: 25%
      maxUnavailable: 25%
    type: RollingUpdate
  template:
    metadata:
      labels:
        app: currency-exchange
    spec:
      containers:
      - image: in28min/mmv2-currency-exchange-service:0.0.11-SNAPSHOT
        imagePullPolicy: IfNotPresent
        name: mmv2-currency-exchange-service
      restartPolicy: Always
---
apiVersion: v1
kind: Service
metadata:
  labels:
    app: currency-exchange
  name: currency-exchange
  namespace: default
spec:
  ports:
  - port: 8000
    protocol: TCP
    targetPort: 8000
  selector:
    app: currency-exchange
  sessionAffinity: None
  type: LoadBalancer
```


# Enabling Logging and Tracing API's in GCP 
- GCP has it's own versions of Logging and tracing services (Like Zipkin)
- Go to GCP
- Search "API's & Services"
- Click on "ENABLE APIS AND SERVICES"
- Search for "Cloud Logging API"

![image](https://user-images.githubusercontent.com/36638342/147841272-15cb80f4-23dc-4b52-bbd4-4adb691dbb34.png)

![image](https://user-images.githubusercontent.com/36638342/147841278-10496521-f9f8-4050-851f-3dbabb9e2305.png)

- Enable it

For tracing

- Go to GCP
- Search "API's & Services"
- Click on "ENABLE APIS AND SERVICES"
- Search for "Stack"
- Enable the 5 services in the results
  - Stackdriver API
  - Stackdriver Monitoring
  - Stackdriver Trace
  - Stackdriver Error Reporting
  - Stackdriver Profiler API  


# Create Environment variables to enable Microservices Communication
- Its better to create custom environment variables instead of using the default ones for Kubernetes.
- Change in CurrencyExchangeProxy class the name of the environment variable
  - From CURRENCY_EXCHANGE_SERVICE_HOST to CURRENCY_EXCHANGE_URI
- Change the version of the SNAPSHOT in the pom.xml to ``0.0.12``
- Create and push the Docker image to Docker Hub


Exmaple of the YAML
![image](https://user-images.githubusercontent.com/36638342/147841595-0ee56ef7-48e1-4be5-926e-3aa7b30cf2ea.png)


# Configure Centralized and mapping configuration in Kubernetes
- In the Terminal execute ``kubectl create configmap currency-conversion --from-literal=CURRENCY_EXCHANGE_URI=http://currency-conversion``
  - This will map that environmet variable with that value for the currency conversion service

![image](https://user-images.githubusercontent.com/36638342/147841642-951fe82c-0c2c-45ce-8160-9f824b7d6d99.png)


- Execute ``kubectl get configmap`` to get all the mappings
![image](https://user-images.githubusercontent.com/36638342/147841644-ec758c27-6537-4a20-ba2e-e62c655e74ec.png)

- Execute ``kubectl get configmap currency-conversion -o yaml >> configmap.yaml`` to create a file with the configmap of the currency conversion
- Paste it in the deployment file, remeber, separated  with "---"

``deployment-03-final.yaml``
![image](https://user-images.githubusercontent.com/36638342/147841675-334147ce-c56f-4606-bab0-80d83318e3cc.png)


# Deployments of Microservices in Kubernetes with Zero Downtime
- Lets supose we have a wrng deployment, we can rollout to an earlier version.
- ``kubectl rollout history deployment currency-exchange`` to see the list of versions.
- ``kubectl rollout undo deployment currency-exchange --to-revision=1`` to rollback to earlier version

![image](https://user-images.githubusercontent.com/36638342/147841787-13b78680-45d9-431a-afcc-c3a7ce2cd291.png)

## Configuring Liveness and Readiness in Kubernetes
- When we are moving from one version to another we want Zero downtime, then we need to configure Liveness and Readiness.
- If Readiness probe is not successfull, K8s don't send any traffic
- If Liveness probe is not successfull, Pod is restarted.
- We have a couple of urls for those probes.
- Came with the Actuator configuration we did in the properties file

http://34.134.106.231:8000/actuator/health/liveness
http://34.134.106.231:8000/actuator/health/readiness

![image](https://user-images.githubusercontent.com/36638342/147841835-9cb0a721-bee6-4851-877c-59756acef2be.png)

![image](https://user-images.githubusercontent.com/36638342/147841866-d5da5edb-bb96-41a1-b84f-f957dc57ddf7.png)

![image](https://user-images.githubusercontent.com/36638342/147841867-54fb74b9-3b0e-4d3b-a3f5-0c1ec41d1786.png)


- We can configure them in the YAML file, in the container section

![image](https://user-images.githubusercontent.com/36638342/147841887-153d9dc2-ea64-4d3c-9e77-df9ced3347ed.png)



