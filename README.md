# AWS-EKS

<img src="https://github.com/NEU-CSYE7220-DevOps/AWS-EKS/blob/main/EKS-img.jpg" width="100">

## Introduction
* Amazon Elastic Kubernetes Service (EKS) is a managed Kubernetes service that makes it easy for you to run Kubernetes on AWS and on-premises. Amazon EKS is certified Kubernetes conformant, so existing applications that run on upstream Kubernetes are compatible with Amazon EKS.

* Sentiment Analysis Application is deployed in EKS.

* Sentiment Analysis is an important part of bot-based enterprise apps, Capturing sentiment critical to a smooth-running enterprise. This project creates pods for frontend and backend for each deployment with the ability to auto scale automatically. Services with load balancers are used to connect to different pods through service url.

## WorkFlow

* User enters sa-webapp sevice url in text box.
* Browser requests sentiment on button click.
* User triggers requests to a Spring based WebAPI app in different pod.
* Spring WebAPI forwards requests for sentiment analysis to Python app.
* Python WebAPI calculates sentiment and returns the result as a response to Spring Web app.
* Spring Web returns response to the React app, which then presents the information to the user.

## Setup

* Clone the repo from here -> git@github.com:NEU-CSYE7220-DevOps/AWS-EKS.git

## Prerequisites

* Install minikube – https://kubernetes.io/docs/tasks/tools/install-minikube/
* Install kubectl – https://kubernetes.io/docs/tasks/tools/install-kubectl/
* Install brew on the Mac, chocolatey on Windows

## How to Run?

* Start minikube ```minikube start```

* Run Terraform commands 
```
terraform init
terraform plan
terraform apply
```
* Run below commands for Kube configuration
```
aws eks update-kubeconfig --name `<cluster-name>` --region `<region>`
terraform output config_map_aws_auth > ./config-map-aws-auth.yaml
kubectl apply -f ./config-map-aws-auth.yaml

```

* Navigate to Kubernetes/sa-frontend and below commands to create pods, service and deployment of Frontend
  - kubectl create -f service-sa-frontend-lb.yaml
  - kubectl apply -f sa-frontend-deployment-minikube2.yaml

* Navigate to Kubernetes/sa-webapp and below commands to create pods, service and deployment of Webapp
  - kubectl create -f service-sa-webapp-lb.yaml
  - kubectl apply -f sa-webapp-deployment.yaml

* Navigate to Kubernetes/sa-logic and below commands to create pods, service and deployment of sa-logic
  - kubectl create -f service-sa-logic.yaml
  - kubectl apply -f sa-logic-deployment.yaml

* To stop minikube - ```minikube stop```

## Test Resources

* To get Running pods -kubectl get pods
* Get services - kubectl get services
* Get deployments - kubectl get deployments
* Delete resources
  - kubectl delete pod `<pod-name>`
  - kubectl delete service `<service-name>`
  - kubectl delete deployment `<deployment-name>`

## Test application

* Run ``` minikube service list```
* Open sa-frontend-lb link to open application
* Test webapp at `<sa-webapp-lb url>`/testHealth and `<sa-webapp-lb url>`/testComms
* Test sa-logic at `<sa-logic-lb url>`/testHealth and `<sa-logic-lb url>`/testComms
