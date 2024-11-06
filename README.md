# Testing Guide for SRE Instrumentation Solution

- This guide will walk you through testing the setup for the Storage API with Prometheus and Grafana instrumentation.
- *Please read [ImplementationDetails.md](./ImplementationDetails.md) to understand how I implemented the challenge and why I took which decisions.*

## Prerequisites
- Docker
- Docker-compose
- Helm
- Kubectl
- Access to a Kubernetes cluster

### Step 1: Testing Locally with Docker Compose

1. **Start Everything Up**:
   - Run `docker-compose up` to start the Storage API, Prometheus, and Grafana.

2. **Generate Traffic**:
   - To simulate traffic, run:
     ```bash
     ./scripts/generate_traffic.sh
     ```
3. **Access Grafana**:
   - Grafana: [http://localhost:3000](http://localhost:3000) (default login: `admin/admin`)

4. **Set Up the Grafana Dashboard**:
   - Import the provided JSON file **storage_api_grafana_dashboard.json** in Grafana.
   - You should see both panels for:
     - **Average HTTP Request Duration**
     - **HTTP Status Codes**

5. **Clean Up**:
 - To clean up, run:
    ```bash
    docker-compose down
     ```

### Step 2: Testing on Kubernetes with Helm

1. **Deploy with Helm**:
   - Install the Helm chart on your Kubernetes cluster:
     ```bash
     helm install storage-api chart
     ```

2. **Confirm Everything is Running**:
   - Make sure all the pods are up:
     ```bash
     kubectl get pods
     ```
   - You should see pods for the Storage API, Prometheus, and Grafana.

4. **Generate Traffic**:
   - Forward local ports to access the storage API:
     ```bash
     kubectl port-forward svc/storage-api 5000:5000 
     ```
   - To simulate traffic, run 
     ```bash
     ./scripts/generate_traffic.sh
     ```
3. **Access Grafana**:
   - Forward local ports to access them:
     ```bash
     kubectl port-forward svc/storage-api-grafana 3000:80 
     ```
   - Grafana: [http://localhost:3000](http://localhost:3000) (default login: `admin/admin`)

5. **Set Up the Grafana Dashboard**:
   - Import the provided JSON file **storage_api_grafana_dashboard.json** in Grafana.
   - You should see both panels for:
     - **Average HTTP Request Duration**
     - **HTTP Status Codes**
   
6. **Clean Up**:
   - To clean up, run:
    ```
    helm uninstall storage-api
    ```