# Google Cloud Fundamentals: Getting Started with GKE

## Objectives

    - Provision a Kubernetes cluster using Kubernetes Engine.

    - Deploy and manage Docker containers using kubectl

## Steps

1. Sign in to the Google Cloud Platform (GCP) Console

2. Confirm/Enable that needed APIs are enabled 

    - Check if the Kubernetes Engine API and the Container Registry API are enabled/available

        gcloud services list --enabled

    - If you see them on the list then proceed to the next step. If not then enable the required API

        gcloud services enable container.googleapis.com

        gcloud services enable containerregistry.googleapis.com

3. Start a Kubernetes Engine cluster, deploy and expose nginx 

    - Export an environment variable for zone

        export MY_ZONE=us-central1-a

    - Start a Kubernetes cluster managed by Kubernetes Engine. Name the cluster webfrontend and configure it to run 2 nodes

        gcloud container clusters create webfrontend --zone $MY_ZONE --num-nodes 2

    - Check your installed version of Kubernetes using the kubectl version command
    
        kubectl version

    - Launch a single instance of the nginx container

        kubectl create deploy nginx --image=nginx:1.17.10

    - View the pod running the nginx container

        kubectl get pods 

    - Expose the nginx container to the Internet

        kubectl expose deployment nginx --port 80 --type LoadBalancer

    - View the new service:

        kubectl get services

    - Open a new web browser tab and paste your cluster's external IP address into the address bar. 
    Result: The default home page of the Nginx browser is displayed.

    - Scale up the number of pods running on your service

        kubectl scale deployment nginx --replicas 3

    - Confirm that Kubernetes has updated the number of pods

        kubectl get pods

    - Confirm that your external IP address has not changed

        kubectl get services

    - Return to the web browser tab in which you viewed your cluster's external IP address and refresh the page
    Result: The nginx web server should still respond

## Congratulations!

You've just:
- configured a Kubernetes cluster in Kubernetes Engine
- populated the cluster with several pods containing an application
- exposed the application; and 
- scaled the application
(- and become a legend in the making...!!!)
