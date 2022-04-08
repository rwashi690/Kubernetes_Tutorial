# Kubernetes (K8) Tutorial
This GitHub repository is to document my self taught journey into learning Kubernetes through a variety of sources.

## Kubernetes Definition and Purpose
An open source container tool developed by Google that helps you manage applications and deploy them in different environments cloud, physical, virtual, or hybrid. There was a rise of Microservices and lead to increase usage of containers. These applications can be comprised of 1000s of containers and managing them with scripts is really hard. Orchestration tools like Kubernetes offer the following features
* High Availability (your application will never have downtime)
* Easy Scalability (adjust to user traffic by increasing and decreasing load)
* Backup and Restoration (restore easily to the latest state)

## Basic Kubernetes Components
* Node -> A simple server that holds the Pod (virtual or physical machine)
* Pod -> Abstraction over a container and the smallest possible unit in K8. Can be a database or the JS app just runs a single application in general. Each pod has an IP address where each pod can interact with each other. If a pod application terminates the IP address with be re-created on the restart. Thus, if you want to communicate with the database via IP address you would have to readjust your code.
* Service -> To avoid the change in the IP address upon Pod re-creation a permanent IP address or a service is attached to each Pod. Additionally, a Service is used as a load balancer because a duplicate apps are created on multiple pods in case one Pod is busy or a Pod crashes. So the Service will direct the user to the appropriate Pod based on current app traffic.
    * External Service -> makes your application accessible to the public through the browser (typically a string of numbers or the node IP address with the port number for example https://124.89.101.2:8080)
    * Interval Serice -> to avoid a database being available to the public you specify an interval service 
* Ingress -> does the forwarding to the external service which is the actual domain name of the application (example: https://app.com)
* ConfigMap -> contains the external configuration of your applicatgion such as URL of the database or other utilized services of the app. It is connected to the pod to receive the data the ConfigMap contains. If you change the name of the endpoint of the service you can just adjust the ConfigMap. 
* Secret -> Usernames and passwords are also part of the database but ConfigMap is not secure even though ConfigMap is an External Service so they instead go into Secret. Secret will encode the data using base64. This is not enabled by default but needs to be used if user login and credentials are necessary.

Another issue with Kubernetes is that the database is stored within the Pod. This can be a problem because if the pod shuts down the database data is deleted. Thus, **Volumes** are used. Volumes are used to store the database off the K8 cluster on an remote server or local storage. K8s cluster does not manage data persistence instead you must backup the data appropriately on a external source.

In practice Pods are not created by users but rather you create **Deployments** where they allow you to create a blue print for the Pods that will contain your application. However, the database is not replicated with the Deployment due to the database having a state so to invoid inconsistencies another component called **StatefulSet**. 
* Deployment for state**less** apps
* StatefulSet for state**full** apps and databases

[A visualization of K8 from Source 1](7329A881-D93A-4FDF-8689-2B626E1A0557_1_201_a.jpeg)

## Kubernetes Architecture

In general there is at least one master node (or control plane node) and several worker nodes where your applications runs. The master node runs,
* API (application programming interface) Server or the entrypoint to the K8 cluster
* Controller Manager which keeps of what is running on the cluster
* Scheduler that determines what worker node should be used and when a Pod neds to be replaced
* ETCD storage or the K8 backup store for the cluster that contains all the configuration data and the current state of the K8 cluster. The entire cluster state can be recovered through a ETCD snapshot.

How the master and worker nodes communicate is through the **Virtual Network**. Due to the significant number of processes that worker nodes complete they tend to have higher workloads and thus require greater resources than master nodes. However, losing a master node is more damaging than a worker node because without a master node the cluster will not run and the application will not be accessible to the user. Where if a worker nodes crashes the user will have access to the application via another worker node. Therefore, in practice the master node is backed up and in operation there is at least 2 master nodes.

## Install Kubernetes on MacOs
I used [Homebrew](https://brew.sh/) to install kubectl which is the command-line tool for K8. The following lines allowed me to successfully install kubectl on my mac.
```linux
brew install kubectl
```
then to test that installation was successfull
```linux
kubectl version --client
```
I also installed Docker on my mac via the [Docker Website](https://www.docker.com/)

I installed minikube on my Mac also using Homebrew using the following line,
```linux
brew install minikube
```

I was then able to start a cluster by running the following line,
```linux
minikube start
```

## Sources
1. [Youtube Video 1 by TechWorld with Nana] (https://www.youtube.com/watch?v=X48VuDVv0do)
2. [Youtube Video 2 by TechWorld with Nana] (https://www.youtube.com/watch?v=s_o8dwzRlu4)
3. [Kubernetes website] (https://kubernetes.io/docs/home/)