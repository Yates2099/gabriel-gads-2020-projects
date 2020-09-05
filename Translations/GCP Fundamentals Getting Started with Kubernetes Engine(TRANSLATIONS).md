# Overview In this lab, you create a Google Kubernetes Engine cluster containing several containers, each containing a web server. You place a load balancer in front of the cluster and view its contents.

# Objectives
    _In this lab, you learn how to perform the following tasks:_

    _Provision a Kubernetes cluster using Kubernetes Engine._

    _Deploy and manage Docker containers using kubectl._

* Task 1: Sign in to the Google Cloud Platform (GCP) Console ; For each lab, you get a new GCP project and set of resources for a fixed time at no cost.

* Task 2: Confirm that needed APIs are enabled
    ## Make a note of the name of your GCP project. This value is shown in the top bar of the Google Cloud Platform Console. It will be of the form qwiklabs-gcp- followed by hexadecimal numbers.
    _In the GCP Console, on the Navigation menu (Navigation menu), click APIs & Services._

    _Scroll down in the list of enabled APIs, and confirm that both of these APIs are enabled:_

        Kubernetes Engine API
        Container Registry API

    If either API is missing, click Enable APIs and Services at the top. Search for the above APIs by name and enable each for your current project. (You noted the name of your GCP project above.)

    _In GCP console, on the top right toolbar, click the Open Cloud Shell button._
    _Click Continue. cloudshell_continue.png_
    _For convenience, place the zone that Qwiklabs assigned you to into an environment variable called MY_ZONE. At the Cloud Shell prompt, type this partial command_

        export MY_ZONE=

    ## followed by the zone that Qwiklabs assigned to you. Your complete command will look similar to this:

        export MY_ZONE=us-central1-a

    * Task 3: Start a Kubernetes cluster managed by Kubernetes Engine. Name the cluster webfrontend and configure it to run 2 nodes:

        gcloud container clusters create webfrontend --zone $MY_ZONE --num-nodes 2

    _It takes several minutes to create a cluster as Kubernetes Engine provisions virtual machines for you._

    ## After the cluster is created, check your installed version of Kubernetes using the kubectl version command:

        kubectl version

    _The gcloud container clusters create command automatically authenticated kubectl for you._

    ## View your running nodes in the GCP Console. On the Navigation menu (Navigation menu), click Compute Engine > VM Instances.

    _Your Kubernetes cluster is now ready for use_

    * Task 4: Run and deploy a container

    ## From your Cloud Shell prompt, launch a single instance of the nginx container. (Nginx is a popular web server.)

        kubectl create deploy nginx --image=nginx:1.17.10

    _In Kubernetes, all containers run in pods. This use of the kubectl create command caused Kubernetes to create a deployment consisting of a single pod containing the nginx container. A Kubernetes deployment keeps a given number of pods up and running even in the event of failures among the nodes on which they run. In this command, you launched the default number of pods, which is 1_

    ## View the pod running the nginx container:

        kubectl get pods

     ## Expose the nginx container to the Internet:

        kubectl expose deployment nginx --port 80 --type LoadBalancer
    
    ## View the new service:

        kubectl get services

    _It may take a few seconds before the External-IP field is populated for your service. This is normal. Just re-run the kubectl get services command every few seconds until the field is populated._
    _Open a new web browser tab and paste your cluster's external IP address into the address bar. The default home page of the Nginx browser is displayed._

    ## Scale up the number of pods running on your service:

        kubectl scale deployment nginx --replicas 3

    _Scaling up a deployment is useful when you want to increase available resources for an application that is becoming more popular._

    ## Confirm that Kubernetes has updated the number of pods:

        kubectl get pods

    ## Confirm that your external IP address has not changed:

        kubectl get services

    _Return to the web browser tab in which you viewed your cluster's external IP address. Refresh the page to confirm that the nginx web server is still responding._


