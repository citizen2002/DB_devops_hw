# HW on kubernetes monitoring
Simple documentation for setting up monitoring kubernetes and achieving our goals <br>
**This documentation is designed for Ubuntu Linux 22.04**<br>
**This repository will be automatically closed after 2 weeks to prevent leak of this HW**
# Table of Contents
+ [Prerequisite - VM machine requirements](https://github.com/citizen2002/DB_devops_hw#prerequisite---vm-machine-requirements)
+ [Technologies that we will be using](https://github.com/citizen2002/DB_devops_hw#technologies-that-we-will-be-using)
1. [Project installation (skip this part if you have technologies already installed)](https://github.com/citizen2002/DB_devops_hw#project-installation-skip-this-part-if-you-have-technologies-already-installed)
2. [Project deployment](https://github.com/citizen2002/DB_devops_hw#project-deployment)
3. [Endword/conclusion](https://github.com/citizen2002/DB_devops_hw#endwordconclusion)
## Prerequisite - VM machine requirements
Minimal <br> 
* 2 CPUs or more<br>
* 2GB of free memory<br>
* 20GB of free disk space<br>

Recommended <br> 
* 4 CPUs or more<br>
* 8GB of free memory<br>
* 30GB of free disk space<br>

## Technologies that we will be using
[Docker](https://docs.docker.com/engine/install/ubuntu/ "Docker main") - as a driver for minikube <br>
[Kubectl](https://kubernetes.io/docs/tasks/tools/ "Kubectl main") - to control kubernetes clusters <br>
[Minikube](https://minikube.sigs.k8s.io/docs/start/ "Minikube main") - to control kubernetes clusters <br>
[ArgoCD](https://argo-cd.readthedocs.io/en/stable/getting_started/ "ArgoCD main") - for automation <br>
[Prometheus](https://prometheus.io/docs/prometheus/latest/installation/ "Prometheus main") - for monitoring<br>
[Grafana](https://grafana.com/docs/grafana/latest/setup-grafana/installation/ "Grafana main") - for monitoring<br>

# Project installation (skip this part if you have technologies already installed)
## Docker
Docker will be first thing that we will be installing. <br>
Open Linux terminal and copy/type following commands: <br>
Update your existing list of packages: <br>
```
sudo apt update
```
Next, install a few prerequisite packages which let apt use packages over HTTPS: <br>
```
sudo apt install apt-transport-https ca-certificates curl software-properties-common
```
Then add the GPG key for the official Docker repository to your system: <br>
```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```
Add the Docker repository to APT sources: <br>
```
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"
```
Install Docker: <br>
```
sudo apt install docker-ce
```
Check that it’s running: <br>
```
sudo systemctl status docker
```
## Kubectl
Up next - Kubectl. <br>
Open Linux terminal and copy/type following commands: <br>
Download latest version of kubectl: <br>
```
curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl
```
Make the Kubectl binary executable: <br>
```
chmod +x kubectl
```
Move Kubectl binary to PATH: <br>
```
sudo mv kubectl /usr/local/bin
```
Check that it is running: <br>
```
kubectl version -o yaml
```
## Minikube
Up next - Minikube. <br>
Open Linux terminal and copy/type following commands: <br>
Apply all updates of existing packages of your system by executing the following apt commands: <br>
```
sudo apt update -y
sudo apt upgrade -y
```
Install the following minikube dependencies: <br>
```
sudo apt install -y curl wget apt-transport-https
```
Use the following curl command to download latest minikube binary: <br>
```
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
```
Once the binary is downloaded, copy it to the path /usr/local/bin and set the executable permissions on it: <br>
```
sudo install minikube-linux-amd64 /usr/local/bin/minikube
```
Verify the minikube version:
```
minikube version
```
Start minikube: <br>
```
minikube start --driver=docker
```
(Optional) Access dashboard: <br>
```
minikube dashboard
```
## ArgoCD on to Minikube
Open new terminal <br>
Create a namespace for argocd: <br>
```
kubectl create ns argocd
```
Apply ArgoCD manifest installation file from ArgoCD github repository: <br>
```
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/v2.5.8/manifests/install.yaml
```
Verify the installation: <br>
```
kubectl get all -n argocd
```
## Accessing ArgoCD's interface
Do a port forwarding: <br>
```
kubectl port-forward svc/argocd-server -n argocd 8080:443
```
Go to a browser and open localhost:8080 - username !!!Always ***admin***!!!<br>
Open a new terminal and enter the following code to access user:admin password: <br>
```
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo
```
Copy the password, go back to your browser and enter it as password <br>
## Prometheus
Install Helm: <br>
```
sudo apt-get install helm --classic
```
Download Prometheus: <br>
```
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
```
```
helm repo add grafana https://grafana.github.io/helm-charts
helm repo update
```
Install Prometheus: <br>
```
helm install prometheus prometheus-community/prometheus
```
Create a new service using nodeport: <br>
```
kubectl expose service prometheus-server --type=NodePort --target-port=9090 --name=prometheus-server-new
```
Access promethus gui: <br>
```
minikube service prometheus-server-new
```
Install grafana in a new terminal: <br>
```
helm install grafana grafana/grafana
```
Create a new service with nodeport: <br>
```
kubectl expose service grafana --type=NodePort --target-port=3000 --name=grafana-new
```
Access grafana dashboard: <br>
```
minikube service grafana-new
```
Open new terminal to acquire password for Grafana: <br>
```
kubectl get secret --namespace default grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
```
Login with ***admin*** user and acquired password <br>
# Project deployment
## Minikube cluster deployment
Set up deployment: <br>
```
kubectl apply -f devops-hw.yaml
```
Create service: <br>
```
kubectl apply -f devops-hw-service.yaml
```
Access the application: <br>
```
minikube service devops-hw --url
```
### Minikube stress test (manual)
Enable a Metrics Server: <br>
```
minikube addons enable metrics-server
```
Autoscale deployment for CPU: <br>
```
kubectl autoscale deployment devops-hw --cpu-percent=90 --min=1 --max=4
```
Deploy this YAML file: <br>
```
kubectl apply -f nuke.yml
```
Execute container (somewhere here, I think, I made a mistake): <br>
```
kubectl exec -it devops sh
```
## Github to ArgoCD automation stack
Connect ArgoCD to [current](https://github.com/citizen2002/DB_devops_hw) repository via HTTPS (recommended) or SSH methods following [this](https://argo-cd.readthedocs.io/en/stable/user-guide/private-repositories/) tutorial (first 3 paragraphs) <br>
## Prometheus monitoring
Connect to Prometheus dashboard. <br>
Create a new namespace: <br>
```
kubectl create namespace monitoring
```
Create role: <br>
```
kubectl create -f clusterRole.yaml
```
Create the config map in Kubernetes: <br>
```
kubectl create -f config-map.yaml
```
Create a deployment on monitoring namespace: <br>
```
kubectl create  -f prometheus-deployment.yaml 
```
### Prometheus high CPU alert
Create a namespace: <br>
```
kubectl create namespace my-alert
```
Send alert.yaml to server: <br>
```
kubectl apply -f alert.yaml --namespace=my-alert
```
## Grafana monitoring
Create a namespace: <br>
```
kubectl create namespace my-grafana
```
Send grafana.yaml to server: <br>
```
kubectl apply -f grafana.yaml --namespace=my-grafana
```
Access it by exposing Grafana IP: <br>
```
minikube service grafana --namespace=my-grafana
```
### Telegram alert via Telepush
Obtain your own [Telepush](https://telepush.dev/) token by clicking on "Telepush" <br>
Configure ***alerttele.yml*** by inserting your token in 13th code line <br>
Apply the configuration: <br>
```
curl -X POST http://localhost:9093/-/reload
```
(Optional) - ***Configure prometheus.yml***, ***custom_alerts.yml*** and ***blackbox.yml*** in the lines with the comments <br>

# Endword/conclusion
Even though some of my ideas and implementations didn't work out the way i wanted them to I have really enjoyed this challenge. I went all in into it and it helped me understand where are my knowledge/practical gaps and as well learned many new devops tools. I blame my high confidence thinking that this project would've been much easier and starting it much later than I really should have.<br>
***THANK YOU***