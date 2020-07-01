Kotlin project to deploy on Kubernetes using Jenkins
===
Example project on how to automatically deploy a Kotlin REST service on Kubernetes using a Jenkins pipeline. 

Requirements
---
- Kotlin
- Maven
- Docker

Install Helm
---
```curl https://helm.baltorepo.com/organization/signing.asc | sudo apt-key add -
sudo apt-get install apt-transport-https --yes
echo "deb https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
sudo apt-get update
sudo apt-get install helm```

```sudo ln -s /usr/sbin/helm /usr/local/bin/helm
```


Build
---
1. Build project
    ```
    mvn clean package
    ```
1. Build Docker image 
    ```
    docker build -t k8s-jenkins-example .
    ```
    
    
