#LAB: Google Cloud Fundamentals: Getting Started with Compute Engine

##Objectives:

In this lab, you will learn how to perform the following tasks:

 - Create a Compute Engine virtual machine using the Google Cloud Platform (GCP) Console.

 - Create a Compute Engine virtual machine using the gcloud command-line interface.

 - Connect between the two instances.

## Steps:

1. Create a Compute Engine virtual machine using the Google Cloud Platform (GCP) Console.

    gcloud compute instances create my-vm-1 --machine-type "n1-standard-1" --image-project "debian-cloud" --image "debian-9-stretch-v20190213" --subnet "default" --tags http

  - Add firewall rule for the tags created above
    
    gcloud compute firewall-rules create default-allow-http --direction=INGRESS --action=ALLOW --rules=tcp:80 --target-tags=http

2. Create a virtual machine using the gcloud command line

    gcloud config set compute/zone us-central1-b

    gcloud compute instances create my-vm-2 --machine-type "n1-standard-1" --image-project "debian-cloud" --image "debian-9-stretch-v20190213" --subnet "default"

3. Connect between VM instances 

    - Connect to my-vm-2

        gcloud compute ssh my-vm-2
    
    - Use the ping command to confirm that my-vm-2 can reach my-vm-1 over the network:
        
        ping  -c 4 my-vm-1

    
    //This  does not work for me in the lab, error is 'ERROR: (gcloud.compute.ssh) Could not fetch resource:
    - Insufficient Permission: Request had insufficient authentication scopes. FIND SOLUTION.
    //If this doesn't work ssh directly into my-vm-1
    - Use the ssh command to open a command prompt on my-vm-1 (from my-vm-2)

        ssh my-vm-1 

    - At the command prompt on my-vm-1, install the Nginx web server

        sudo apt-get install nginx-light -y

    - Use the nano text editor to add a custom message to the home page of the web server

        sudo nano /var/www/html/index.nginx-debian.html

    - Use the arrow keys to move the cursor to the line just below the h1 header. Add your name

        Hi from Graham

    - Press Ctrl+O and then press Enter to save your edited file, and then press Ctrl+X to exit the nano text editor

    - Confirm that the web server is serving your new page. At the command prompt on my-vm-1, execute this command

        curl http://localhost/

        - The response will be the HTML source of the web server's home page, including your line of custom text.    

    - Exit the command prompt on my-vm-1

        exit

    - Confirm that my-vm-2 can reach the web server on my-vm-1, at the command prompt on my-vm-2, execute this command

        curl http://my-vm-1/

        - The response will be the HTML source of the web server's home page, including your line of custom text.

    - Copy the External IP address for my-vm-1

        gcloud compute instances list --zone us-central1-a

    - Paste it into the address bar of a new browser tab

        Result should be that your web server's home page will be displayed, including your custom text

