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
kubectl expose deployment guestbook --type=LoadBalancer --port=3000

```

7.2 Display Kubernetes services deployed in previous step:
```
kubectl get services guestbook
```
It may take up to 5 min to update Loadbalancer in IBM Cloud with new service. As an example of output:
```
$ kubectl get services guestbook
NAME        TYPE           CLUSTER-IP      EXTERNAL-IP                            PORT(S)          AGE
guestbook   LoadBalancer   172.21.50.119   fb9c40ef-us-south.lb.appdomain.cloud   3000:32334/TCP   41s
```
7.5 Use the loadbalancer in "LoadBalancer Ingress:" field and HTTP port in "TargetPort:". Open your application in a browser at https://"LoadBalancer Ingress:"TargetPort:". Output display should be:
ie. http://fb9c40ef-us-south.lb.appdomain.cloud:3000
```
curl -v http://fb9c40ef-us-south.lb.appdomain.cloud:3000
```
```
* Rebuilt URL to: http://fb9c40ef-us-south.lb.appdomain.cloud:3000/
*   Trying 150.240.68.156...
* TCP_NODELAY set
* Connected to fb9c40ef-us-south.lb.appdomain.cloud (150.240.68.156) port 3000 (#0)
> GET / HTTP/1.1
> Host: fb9c40ef-us-south.lb.appdomain.cloud:3000
> User-Agent: curl/7.61.1
> Accept: */*
> 
< HTTP/1.1 200 OK
< Accept-Ranges: bytes
< Content-Length: 1047
< Content-Type: text/html; charset=utf-8
< Last-Modified: Mon, 10 Dec 2018 00:51:16 GMT
< Date: Wed, 06 Apr 2022 18:24:56 GMT
< 
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta content="text/html; charset=utf-8" http-equiv="Content-Type">
    <meta charset="utf-8">
    <meta content="width=device-width" name="viewport">
    <link href="style.css" rel="stylesheet">
    <title>Guestbook - v1</title>
  </head>
  <body>
    <div id="header">
      <h1>Guestbook - v1</h1>
    </div>

    <div id="guestbook-entries">
      <link href="https://afeld.github.io/emoji-css/emoji.css" rel="stylesheet">
      <p>Waiting for database connection... <i class='em em-boat'></i></p>
      
    </div>

    <div>
      <form id="guestbook-form">
        <input autocomplete="off" id="guestbook-entry-content" type="text">
        <a href="#" id="guestbook-submit">Submit</a>
      </form>
    </div>

    <div>
      <p><h2 id="guestbook-host-address"></h2></p>
      <p><a href="env">/env</a>
      <a href="info">/info</a></p>
    </div>
    <script src="//ajax.googleapis.com/ajax/libs/jquery/2.1.1/jquery.min.js"></script>
    <script src="script.js"></script>
  </body>
</html>
* Connection #0 to host fb9c40ef-us-south.lb.appdomain.cloud left intact
```

### 8-  Clean up and remove application
	
8.1 List application deployment
```
kubectl get deployments
```
	
8.2 Identify kubernetesnodeapp-deployment application in the list and delete
```
kubectl delete -n default deployment guessbook
```
	
8.3 List services
```
kubectl get services
```
	
8.4 Delete guessbook services
```
kubectl delete -n default service guessbook 
```
	
