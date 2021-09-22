# WORDPRESS TEST SITE DEPLOYMENT
Docker-compose was selected as the technology to deploy the solution because it allows for a faster prototyping experience (no kubernetes cluster or other extra infrastructure is needed and it takes less time to troubleshoot any build errors or integration issues between services), for a  production deployment it is adviced to translate this already tested structure into a Helm chart.

## Build
Since creating a custom Wordpress site was beyond of the scope of this repo, no extra steps are needed to build the application, the _docker-compose_ installs a standard Wordpress site and takes over the installation and activation of extra plugins and other configurations using initialization scripts. If needed, the website can be built beforehand, and, with minor modifications, mounted directly as a _volume_  to the container (or copied to the image at build time and relocated to be served with the use of the initialization script). The individual images can be built beforehand by doing a _docker-compose build_ or when starting the application by doing a _docker-compose up --build_

## Automation
It is recommended to use an automation server (a multibranch pipeline in Jenkins using the Github plugin could be a good fit for this project). This gives the versatility to modify the deployment to take into account future changes, some of the ways the deployment could work are:
    - (Not recommended at all, but still possible) Running the application directly on the automation server 
    -  _ssh-ing_ into the remote server and runnig the application there 
    - Using _docker contexts_ to specify the target remote 
    - If the Helm chart is created it can be deployed to a Kubernetes cluster

In this scenario the pipeline can be defined in a file and commited to the repository to be picked up by the automation server and run every time there is a change. This also allows us to define the build process for the application in a centralized way.

## Configuration/Credentials
The deployment's configuration and credentials can be modified with the use of environment variables (there will be environment variables available inside the containers, which, in turn, will get their values from the hosts' environment variables). In further iterations _docker/kubernetes secrets_  can be used to provide a higher isolation for this sensitive data.

## Monitoring
A Prometheus and Grafana containers are also being created as part of the deployment, some basic integration between  Prometheus and Wordpress through the use of a Wordpress plugins (just a basic metric is in place to showcase this feature). And Prometheus can be set as a _Data Source_ in Grafana to better visualized (this is not configured at the moment _out of the box_ and needs to be done manually) 