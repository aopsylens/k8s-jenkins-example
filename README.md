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
sudo apt-get install helm
sudo ln -s /usr/sbin/helm /usr/local/bin/helm

helm repo add stable https://kubernetes-charts.storage.googleapis.com
```

Install Traefik
---

```
helm install traefik stable/traefik  --values k8s/traefik-values.yml
```
Get ip from lb
```
kubectl describe svc traefik --namespace default | grep Ingress | awk '{print $3}'
```

Add ip from command above and domain traefik.localhost to /etc/hosts

Access to dashboard https://traefik.localhost with creds admin/admin

Install Kubernetes Dashboard
---

```
kubectl create clusterrolebinding kubernetes-dashboard --clusterrole=cluster-admin --serviceaccount=default:kubernetes-dashboard

helm repo add kubernetes-dashboard https://kubernetes.github.io/dashboard/

helm install dashboard kubernetes-dashboard/kubernetes-dashboard --values k8s/dashboard-values.yml
```

Get token from secret to access dashboard

```
kubectl describe secret kubernetes-dashboard-token-***
```
Add ip from lb and domain dashboard.localhost to /etc/hosts

Access to dashboard https://dashboard.localhost with creds admin/admin


Install Jenkins
---

```
helm install jenkins stable/jenkins --values k8s/jenkins-values.yml
```

Add ip from lb and domain jenkins.localhost to /etc/hosts

Access to dashboard https://jenkins.localhost with creds admin/admin

Build
---
1. Build project
    ```
    mvn clean package
    ```
2. Build Docker image 
    ```
    docker build -t k8s-jenkins-example .
    ```
    
Configure Pipeline
---

Create credentials with "kind Username with passowrd" for access to dockerhub and github


