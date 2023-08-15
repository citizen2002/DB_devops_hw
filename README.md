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
[Docker](https://docs.docker.com/engine/install/ubuntu/ "Docker main") <br>
[Kubectl](https://kubernetes.io/docs/tasks/tools/ "Kubectl main") <br>
[Minikube](https://minikube.sigs.k8s.io/docs/start/ "Minikube main") <br>
[ArgoCD](https://argo-cd.readthedocs.io/en/stable/getting_started/ "ArgoCD main") <br>
[Prometheus](https://prometheus.io/docs/prometheus/latest/installation/ "Prometheus main") <br>
[Grafana](https://grafana.com/docs/grafana/latest/setup-grafana/installation/ "Grafana main") <br>

## Project deployment / installation
### Docker
Docker will be first thing that we will be installing. <br>
Open Linux terminal and type following commands: <br>
```
$ sudo apt update
$ sudo apt install apt-transport-https ca-certificates curl software-properties-common
```