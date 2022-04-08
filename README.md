 ### 1- Hands-on lab training guestbook-app-iks-lab

Deploy a guestbook Web App on IKS cluster Workshop 

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


### 5- Deploy the application with kubectl cli
In this section you will deploy the guessbook starter application using kubectl CLI. 

5.1- Define an environment variable named MYPROJECT and set the name of the application by replacing the placeholder with your initials:
```
export MYPROJECT=<your-initials>kubenodeapp
```
2- Identify your cluster:
```
ibmcloud ks cluster ls
```

5.3- Initialize the variable with the cluster ID

```
export MYCLUSTER=<CLUSTER_ID>
```

5.4- Initialize the kubectl cli environment
```
ibmcloud ks cluster config --cluster $MYCLUSTER
```

5.5- You can either use the default Kubernetes namespace or create a new namespace for this application.

If you wish to use the default Kubernetes namespace, run the below command to set an environment variable
```
export KUBERNETES_NAMESPACE=default
```

### 6- Lab 1. Deploy your first application
Note 1: For exposing guestbook application to intenet, use Loadbalancer as type instead of NodePort:
kubectl expose deployment guestbook --type=LoadBalancer --name=guestbook --port 3000

https://ibm.github.io/kube101/Lab1/

### 7- Lab 2: Scale and Update Deployments

https://ibm.github.io/kube101/Lab2/


### 8- Lab 3: Scale and update apps natively, building multi-tier applications
https://ibm.github.io/kube101/Lab3/

