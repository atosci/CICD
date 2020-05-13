# Ondersteunend

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












