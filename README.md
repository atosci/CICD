# CI/CD

<b>Kubernetes</b>

The environment is built with containers, these are suitable to use with Kubernetes (k8s). Therefor you need to have access to an k8s environment. The deployment-files are only tested for the managed Azure Kubernetes Services.  All deployments can be done with the command “kubectl apply -f {filename} ”.
There are three namespaces required. These are equal to the branches (develop, release and master) which are used to separate the applications environments. Jenkins automatically selects the proper namespace with the branch-pipeline. 
For monitoring the Kubernetes deployments and pods it is possible to deploy the “Kubernetes dashboard”. It’s recommended to use the next example and only use it with the k8s proxy. https://github.com/kubernetes/dashboard/blob/master/README.md

<b>Jenkins</b>

Before deploying the Jenkins container, create a persistent volume for this container with this command:
```
kubectl apply -f persistent_vol_jenkins.yaml
```
Next deploy the container and create a LoadBalancer to make it accessible, use the following commands:
```
kubectl apply -f jenkins_deploy.yaml
kubectl apply -f jenkins_service.yaml
```

<b>SonarQube</b>

SonarQube is used to track code quality and security. First, create the four persistent volumes that belong to SonarQube using:
```
kubectl apply -f persistent_vol_sonar_<extension>.yaml
```
Just like with Jenkins, we than need to deploy the container and create a LoadBalancer:
```
kubectl apply -f sonar_deploy.yaml
kubectl apply -f sonar_service.yaml
```

<b>Docker</b>

To be able to ue Docker from within the Jenkins Pipeline, a Docker in Docker (dind) container is needed. This runs a Docker daemon inside a Docker container. Create the dind container and create a service using expose:
```
kubectl apply -f dind.yaml
kubectl expose pod dind --name=dockerapp--port=2375 --target-port=2375
```





# Jenkins installation


Jenkins is installed on a kubernetes cluster on Azure with kubernetes version 1.15.10.

On this cluster, Jenkins is installed on a pod with the atosci/jenkinsdocker:v3 docker image.


## Jenkins configuration

Jenkins is configured with several plugins and different pipelines, we will go into this later on.



# Plugins

## Sonarqube scanner
SonarQube Scanner for Jenkins is a plugin that has to be installed for the sonarqube webhook.
The version currently used is: 4.3.0.2102. This is the latest version at the time of writing this.

## Sonarqube servers
Go into Jenkins -> configuration and configure the sonarqube servers with your URL and authentication token.

## Kubernetes CLI plugin
This plugin enables the use of kubectl within the pipeline. This is done with the following parameters in a pipeline:


## Maven plugin
Also Maven needs to be installed automatically. This will be done with the Maven plugin that is included in the Jenkins installation. We can set this up to be automatically installed in the 'Jenkins-> global configuration' settings. Make sure to use the same version of Maven used inside the Java project.

## Docker plugin
You also need to define the Docker installation in the global tool configuration of Jenkins.
Check the box 'install Docker automatically' with the latest version.
