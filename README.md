 1- Hands-on lab training guessbook-app-iks-lab

Deploy a HTTP Web App on IKS cluster Workshop 

The purpose of this structured, no-cost, hands-on, instructor-led learning 
experience is to demonstrate how to combine different technologies such 
as Container Orchestration Platform, Container Registry, and cloud 
computing environment to kickstart cloud native applications. Specifically, 
the attendees will learn how to: 

     Deploy a guessbook application to the Kubernetes cluster.

IBM Documentation for some other solution tutorials:
https://cloud.ibm.com/docs/solution-tutorials?topic=solution-tutorials-scalable-webapp-kubernetes

## 2- Lab preparation
Follow these instructions for the setup of the initial Lab environment.

1- Create an access group in your IBM Cloud account: "Demo LABs"
https://cloud.ibm.com/iam/groups
Create
Enter "Demo LABs" as name of access group
Enter a short description
Add the following access polices:
	
1.1 VPC Infrastructure Services service
1.2 Cloud Object Storage service
1.3 Security Advisor service
1.4 All resource group
    resourceType string equals resource-group
1.5 Toolchain service
1.6 Certificate Manager service
1.7 Schematics service
1.8 All resources in account (including future IAM enabled services)

2- Create a resource group "labs"
https://cloud.ibm.com/account/resource-groups

3- Invite users to "Demo LABs" access group via:
https://cloud.ibm.com/iam/users/invite_users
enter user's email address
select "Demo LABs"

Users need to go to their notification section to accept the invite:
https://cloud.ibm.com/notifications
First time users of ibm cloud need to create a new ibm cloud id. Consult with your ibm contact for first time ID creation ( https://cloud.ibm.com/registration ) 

4- Work with your IKS team to deploy an IKS cluster required for this lab.

## 3- Objectives

Deploy a guessbook application to the Kubernetes cluster.
Monitor the logs and health of the cluster.
Scale Kubernetes pods.

3.1- A developer downloads or clones a starter guessbook application

3.2- The application is deployed to a Kubernetes cluster

3.3- Users access the application


## 4- Start a new IBM Cloud Shell or setup your client
You can use IBM Cloud Shell or setup your laptop client environment.

4.1 From the IBM Cloud console in your browser, select the account where you have been invited.

4.2 Click the button in the upper right corner to create a new Cloud Shell ( https://cloud.ibm.com/shell )

4.3 If you are interested to execute this lap from your laptop , you need to install the following tools in your laptop:
Install must-have tools to be productive with IBM Cloud:

    * IBM Cloud CLI - the command line interface to interact with IBM Cloud API.
    * Docker - to deliver and run software in packages called containers.
    * kubectl - a command line interface for running commands against Kubernetes clusters.
    * oc - manages OpenShift applications, and provides tools to interact with each component of your system.
    * Helm 3 - helps you manage Kubernetes applications — Helm Charts help you define, install, and upgrade even the most complex Kubernetes application.
    * Terraform - automates your resource provisioning.
    * jq - a lightweight and flexible command-line JSON processor.
    * Git - a free and open source distributed version control system.

For tools installation, please follow instructions in this link: https://cloud.ibm.com/docs/solution-tutorials?topic=solution-tutorials-tutorials


### 5- Deploy the application with Helm 3
In this section you will deploy the guessbook starter application using kubectl CLI. 

1- Define an environment variable named MYPROJECT and set the name of the application by replacing the placeholder with your initials:
```
export MYPROJECT=<your-initials>kubenodeapp
```
2- Identify your cluster:
```
ibmcloud ks cluster ls
```

3- Initialize the variable with the cluster name

```
export MYCLUSTER=<CLUSTER_NAME>
```

4- Initialize the kubectl cli environment
```
ibmcloud ks cluster config --cluster $MYCLUSTER
```

5- You can either use the default Kubernetes namespace or create a new namespace for this application.

If you wish to use the default Kubernetes namespace, run the below command to set an environment variable
```
export KUBERNETES_NAMESPACE=default
```

6- Install the guessbook app:
```
git clone https://github.com/IBM/guestbook.git
```
```
kubectl create deployment guestbook --image=ibmcom/guestbook:v1

```
7- Display hello-world app deployment:
```
kubectl get deployments guestbook
kubectl describe deployments guestbook
```

8- Display hello-world ReplicaSet objects:
```
kubectl get pods
kubectl get replicasets
kubectl describe replicasets
```

### 7-  Use the IBM-provided domain for your cluster
Paid clusters come with an IBM-provided domain. This gives you a better option to expose applications with a proper URL and on standard HTTP/S ports.

Create a LoadBalancer that exposes the hello-world deployment:

7.1 Identify your IBM-provided Ingress domain
```
ibmcloud ks cluster get --cluster $MYCLUSTER
```
Deploy the service:
```
kubectl expose deployment guestbook --type=oadBalancer --port=3000

```

7.2 Display Kubernetes services deployed in previous step:
```
kubectl get services guestbook
```
It may take up to 5 min to update Loadbalancer in IBM Cloud with new service. As an example of output:
```
Name:                     my-service
Namespace:                default
Labels:                   app.kubernetes.io/name=load-balancer-example
Annotations:              <none>
Selector:                 app.kubernetes.io/name=load-balancer-example
Type:                     LoadBalancer
IP Families:              <none>
IP:                       172.21.72.4
IPs:                      172.21.72.4
LoadBalancer Ingress:     f17a4a30-us-south.lb.appdomain.cloud
Port:                     <unset>  8080/TCP
TargetPort:               8080/TCP
NodePort:                 <unset>  32691/TCP
Endpoints:                172.17.32.206:8080,172.17.32.207:8080,172.17.40.205:8080 + 2 more...
Session Affinity:         None
External Traffic Policy:  Cluster
Events:
  Type    Reason                           Age   From                Message
  ----    ------                           ----  ----                -------
  Normal  EnsuringLoadBalancer             15m   service-controller  Ensuring load balancer
  Normal  EnsuredLoadBalancer              15m   service-controller  Ensured load balancer
  Normal  CloudVPCLoadBalancerNormalEvent  10m   ibm-cloud-provider  Event on cloud load balancer my-service for service default/my-service with UID d3026d55-587d-434d-b318-fecd19e32193: The VPC load balancer that routes requests to this Kubernetes LoadBalancer service is currently online/active.
  
```
7.5 Use the loadbalancer in "LoadBalancer Ingress:" field and HTTP port in "TargetPort:". Open your application in a browser at https://"LoadBalancer Ingress:"TargetPort:". Output display should be:
```
Hello Kubernetes!
```

### 8-  Clean up and remove application
	
8.1 List application deployment
```
kubectl get deployments
```
	
8.2 Identify kubernetesnodeapp-deployment application in the list and delete
```
kubectl delete -n default deployment hello-world 
```
	
8.3 List installed ingress controller 
```
kubectl get ingress
```
	
8.4 Delete ngress-for-ibmdomain-http-and-https Ingress controller 
```
kubectl delete -n default service my-service  
```
	