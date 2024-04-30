# Monitoring Kubernetes Cluster with Prometheus and Grafana

In This project we install Kubernates, Prometheus and Grafana from scratch. And use Prometheus to monitor Kubernates cluster.
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
![Start minikube](https://github.com/MOstafaZaRiaa/monitoring-k8s-using-prometheus/blob/main/images/5.PNG?raw=true)

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

### Installation Steps

1. Add Grafana Helm repository:
    ```bash
    helm repo add grafana https://grafana.github.io/helm-charts
    ```

2. Update the Helm repositories:
    ```bash
    helm repo update
    ```

3. Install Grafana using Helm:
    ```bash
    helm install grafana grafana/grafana
    ```

4. Expose Grafana service:
    ```bash
    kubectl expose service grafana --type=NodePort --target-port=3000 --name=grafana-np
    ```

Accessing the Services
You can access Prometheus and Grafana using the NodePort IP at ports 9090 and 3000 respectively.

"minikube ip":9090 # Prometheus

"minikube ip":3000 # Grafana

# Conclusion

Congratulations! You have successfully set up Prometheus and Grafana for monitoring your Kubernetes cluster. With Grafana deployed, you can now configure dashboards to visualize various cluster metrics and gain insights into the health and performance of your Kubernetes environment.

![all up services](https://github.com/MOstafaZaRiaa/monitoring-k8s-using-prometheus/blob/main/images/Capture.PNG?raw=true)

![all up services](https://github.com/MOstafaZaRiaa/monitoring-k8s-using-prometheus/blob/main/images/2.PNG?raw=true)

![grafana](https://github.com/MOstafaZaRiaa/monitoring-k8s-using-prometheus/blob/main/images/3.PNG?raw=true)






