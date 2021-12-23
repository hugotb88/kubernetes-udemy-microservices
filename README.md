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


