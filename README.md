# HW on kubernetes monitoring
Simple documentation for setting up monitoring kubernetes and achieving our goals <br>
_**This documentation is designed for Ubuntu Linux 22.04**_
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

## Project deployment / installation
### Docker
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
(Optional) Reboot (just in case): <br>
```
sudo reboot
```
### Kubectl
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
### Minikube
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