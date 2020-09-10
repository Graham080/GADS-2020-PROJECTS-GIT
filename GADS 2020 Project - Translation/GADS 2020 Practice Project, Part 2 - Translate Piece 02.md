# AK8S-04 Creating a GKE Cluster via Cloud Shell

## Objectives

- Use kubectl to build and manipulate GKE clusters

- Use kubectl and configuration files to deploy Pods

- Use Container Registry to store and deploy containers


## Steps

### Deploy GKE clusters

1. Set the environment variable for the zone and cluster name

    export my_zone=us-central1-a
    export my_cluster=standard-cluster-1

2. The following command creates a Kubernetes cluster

    gcloud container clusters create $my_cluster --num-nodes 3 --zone $my_zone --enable-ip-alias

### Modify GKE clusters

3. Modify standard-cluster-1 to have four nodes

    gcloud container clusters resize $my_cluster --zone $my_zone --size=4

When prompted with Do you want to continue (Y/n), press y to confirm

### Connect to a GKE cluster

4. Create a kubeconfig file with the credentials of the current user and provide the endpoint details for a specific cluster

    gcloud container clusters get-credentials $my_cluster --zone $my_zone

5. Open the kubeconfig file with the nano text editor:

    nano ~/.kube/config

Examine the conents and then press CTRL+X to exit the nano editor.

### Use kubectl to inspect a GKE cluster

6. Print out the content of the kubeconfig file:

    kubectl config view

7. Print out the cluster information for the active context:

    kubectl cluster-info

8. Print out the active context:

    kubectl config current-context

9. Print out some details for all the cluster contexts in the kubeconfig file:

    kubectl config get-contexts

10. Change the active context:

    kubectl config use-context gke_${GOOGLE_CLOUD_PROJECT}_us-central1-a_standard-cluster-1

11. View the resource usage across the nodes of the cluster:

    kubectl top nodes

12. Enable bash autocompletion for kubectl:

    source <(kubectl completion bash)

### Deploy Pods to GKE clusters

Use kubectl to deploy Pods to GKE

13. Deploy the latest version of nginx as a Pod named nginx-1

    kubectl create deployment nginx-1 --image nginx:latest

14. View all the deployed Pods in the active context cluster

    kubectl get pods

15. Use a variable, type your Pod's unique name in place of [your_pod_name].

    export my_nginx_pod=[your_pod_name]

17. Confirm that you have set the environment variable successfully

    echo $my_nginx_pod

18. View the complete details of the Pod you just created.

    kubectl describe pod $my_nginx_pod

### Push a file into a container

19. Open a file named test.html in the nano text editor.

    nano ~/test.html

20. Add the following text (shell script) to the empty test.html file

    <html> <header><title>This is title</title></header>
    <body> Hello world </body>
    </html>

Press CTRL+X, then press Y and enter to save the file and exit the nano editor when finished

21. Place the file into the appropriate location within the nginx container in the nginx Pod to be served statically:

    kubectl cp ~/test.html $my_nginx_pod:/usr/share/nginx/html/test.html

### Expose the Pod for testing

22. Create a service to expose our nginx Pod externally

    kubectl expose pod $my_nginx_pod --port 80 --type LoadBalancer

23. Execute the following command to view details about services in the cluster

    kubectl get services

24. Verify that the nginx container is serving the static HTML file that you copied. Replace [EXTERNAL_IP] with the external IP address of your service that you obtained from the output of the previous step.

    curl http://[EXTERNAL_IP]/test.html

25. View the resources being used by the nginx Pod

    kubectl top pods

### Introspect GKE Pods
Prepare the environment

26. Clone the repository to the lab Cloud Shell

    git clone https://github.com/GoogleCloudPlatformTraining/training-data-analyst

27. Change to the directory that contains the sample files for this lab.

    cd ~/training-data-analyst/courses/ak8s/04_GKE_Shell/

28. Deploy your manifest, execute the following command:

    kubectl apply -f ./new-nginx-pod.yaml

29. See a list of Pods

    kubectl get pods

### Use shell redirection to connect to a Pod

30.  Start an interactive bash shell in the nginx container:

    kubectl exec -it new-nginx /bin/bash

31. In the nginx bash shell, execute the following commands to install the nano text editor

    apt-get update
    apt-get install nano

32. in the nginx bash shell, execute the following commands to switch to the static files directory and create a test.html file:

    cd /usr/share/nginx/html
    nano test.html

33. In the nginx bash shell nano session, type the following text:

    <html> <header><title>This is title</title></header>
    <body> Hello world </body>
    </html>
Press CTRL+X, then press Y and enter to save the file and exit the nano editor

34. in the nginx bash shell, execute the following command to exit the nginx bash shell:

    exit

35. set up port forwarding from Cloud Shell to the nginx Pod (from port 10081 of the Cloud Shell VM to port 80 of the nginx container):

    kubectl port-forward new-nginx 10081:80

36. In the second Cloud Shell session, execute the following command to test the modified nginx container through the port forwarding:

    curl http://127.0.0.1:10081/test.html

### View the logs of a Pod

37. execute the following command to display the logs and to stream new logs as they arrive (and also include timestamps) for the new-nginx Pod:

    kubectl logs new-nginx -f --timestamps

Congrats, you've reached the end of the lab translation!!!