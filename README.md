# Monitoring Kubernetes Cluster with Prometheus and Grafana On Centos 9

This repo provides comprehensive instructions for setting up a monitoring infrastructure for Kubernetes clusters using Prometheus and Grafana. By following the steps outlined below, users can establish a robust monitoring system to track the health, performance, and resource utilization of their Kubernetes environments. Additionally, we implement service discovery within Prometheus to automatically detect and integrate new nodes into the monitoring system, ensuring seamless scalability and comprehensive coverage of the Kubernetes cluster.
## Prerequisites

- Docker
- Minikube
- Helm
- Prometheus

## Installation

### Docker

Remove any old versions:

```bash
sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```
# Install Docker

### Steps

1. Install `yum-utils` package:
    ```bash
    sudo yum install -y yum-utils
    ```

2. Add Docker repository:
    ```bash
    sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
    ```

3. Install Docker packages:
    ```bash
    sudo yum install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
    ```

4. Start Docker service:
    ```bash
    sudo systemctl enable --now docker
    ```
5. Add your user to docker group:
    ```bash
    sudo usermod -aG docker $USER && newgrp docker 
    ```
# Install and Set Up kubectl
### Steps
1. Install kubectl binary with curl
    ```
    curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
    ```
2. Validate the binary (optional), Download the kubectl checksum file
    ```
    curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"
    ```
3. Install kubectl
    ```
    sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
    ```    
# Install Minikube

### Steps
1. Install Kubelet: 
    ```
    wget https://storage.googleapis.com/kubernetes-release/release/v1.13.0/bin/linux/amd64/kubelet
    ```
2. Download Minikube binary:
    ```bash
    curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-latest.x86_64.rpm
    ```

3. Install Minikube:
    ```bash
    sudo rpm -Uvh minikube-latest.x86_64.rpm
    ```

4. Start Minikube cluster:
    ```bash
    minikube start
    ```
    ![Start minikube](https://github.com/MOstafaZaRiaa/monitoring-k8s-using-prometheus/blob/main/images/starting%20minikube.PNG?raw=true)

# Install Helm

### Steps

1. Download Helm installation script:
    ```bash
    curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
    ```

2. Provide execute permission to the script:
    ```bash
    chmod 700 get_helm.sh
    ```

3. Run the Helm installation script:
    ```bash
    ./get_helm.sh
    ```


# Deploy Prometheus
### Steps

1. Add Prometheus Helm repository:
    ```bash
    helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
    ```

2. Update the Helm repositories:
    ```bash
    helm install prometheus prometheus-community/prometheus
    ```

3. Install Prometheus using Helm:
    ```bash
    helm repo update
    ```

4. Install Prometheus :
    ```bash
    helm install prometheus prometheus-community/prometheus
    ```
4. Expose Prometheus service:
    ```bash
    kubectl expose service prometheus-server — type=NodePort — target-port=9090 — name=prometheus-server-ext
    ```
5. Open Web App of Prometheus
    ```
    minikube service prometheus-server-ext
    ```

## Deploy Grafana

### Steps

1. Add Grafana Helm repository:
    ```bash
    helm repo add grafana https://grafana.github.io/helm-charts
    ```

2. Update the Helm repositories:
    ```bash
    helm install grafana grafana/grafana
    ```

3. Install Grafana using Helm:
    ```bash
    helm repo update
    ```

4. Expose Grafana service:
    ```bash
    kubectl expose service grafana — type=NodePort — target-port=3000 — name=grafana-ext
    ```
5. Open Grafana Web APP
    ```
    minikube service grafana-ext
    ```
# Get Grafana Credintials
  -
    ```
    kubectl get secret — namespace default grafana -o yaml
    ```
  - User
    ```
    echo “password_value” | openssl base64 -d ; echo
    ```
  - Password
    ```
    echo “username_value” | openssl base64 -d ; echo
    ```


# Conclusion

Congratulations! You have successfully set up Prometheus and Grafana for monitoring your Kubernetes cluster. With Grafana deployed, you can now configure dashboards to visualize various cluster metrics and gain insights into the health and performance of your Kubernetes environment.

![all up services](https://github.com/MOstafaZaRiaa/monitoring-k8s-using-prometheus/blob/main/images/prometheus.PNG?raw=true)

![grafana](https://github.com/MOstafaZaRiaa/monitoring-k8s-using-prometheus/blob/main/images/grafana.PNG?raw=true)






