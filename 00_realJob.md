
# Setting Up Prometheus as a Data Source for Grafana in Minikube

Setting up Prometheus as a data source for Grafana in Minikube involves a few steps. Here’s a simplified guide to get you started:

### Step 1: Start Minikube

If you haven't already started Minikube, do so with:

```bash
minikube start
```

### Step 2: Install Helm (if not installed)

Helm is a package manager for Kubernetes. You can install it using the following command:

```bash
curl https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 | bash
```

### Step 3: Add the Prometheus Community Helm Chart Repository

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
```

### Step 4: Install Prometheus

You can install Prometheus using Helm:

```bash
helm install prometheus prometheus-community/prometheus
```

This will create a Prometheus instance in your Minikube cluster.

### Step 5: Install Grafana

Now, install Grafana using Helm:

```bash
helm install grafana grafana/grafana
```

### Step 6: Access Grafana

To access Grafana, first, get the admin password:

```bash
kubectl get secret --namespace default grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
```

Then, you can access Grafana using a port-forward:

```bash
kubectl port-forward svc/grafana 3000:80
```

Open your browser and navigate to `http://localhost:3000`. Use `admin` as the username and the password you just retrieved.

### Step 7: Add Prometheus as a Data Source in Grafana

1. **Log in to Grafana**.
2. Go to **Configuration** (the gear icon) on the left sidebar and click on **Data Sources**.
3. Click on **Add data source**.
4. Select **Prometheus** from the list.
5. In the **HTTP URL** field, enter:

   ```
   http://prometheus-server:80
   ```

6. Click **Save & Test** to verify the connection.

### Step 8: Create Dashboards

Now you can start creating dashboards in Grafana using the data from Prometheus!

### Troubleshooting Tips

- If you have trouble accessing services, ensure that Minikube is running properly.
- Use `kubectl get pods` to check the status of your pods.
- Ensure you have sufficient resources allocated to Minikube.

Let me know if you need further assistance!

## Prerequisites

- [Minikube](https://minikube.sigs.k8s.io/docs/start/) installed and running
- [Kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/) configured to use Minikube
- [Helm](https://helm.sh/docs/intro/install/) installed
- [Grafana](https://grafana.com/get) installed or deployed
- Basic understanding of Kubernetes and Helm

## Step 1: Start Minikube

To start Minikube, follow these steps:

1. **Open your terminal** (Command Prompt, PowerShell, or Terminal).

2. **Run the following command**:

   ```bash
   minikube start
   ```

This command initializes your local Kubernetes cluster. It may take a few moments to download the necessary images and start the cluster.

3. **Check the status** (optional):

   After Minikube starts, you can verify the status by running:

   ```bash
   minikube status
   ```

You should see information about the cluster, including its components and whether they are running.

Let me know if you need help with anything else!

## Step 2: Deploy Prometheus

1. **Add the Prometheus Helm repository:**

   ```bash
   helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
   helm repo update
   ```

2. **Install Prometheus using Helm:**

   ```bash
   helm install prometheus prometheus-community/prometheus --namespace monitoring --create-namespace
   ```

3. **Verify the installation:**

   Check that the Prometheus pods are running:

   ```bash
   kubectl get pods -n monitoring
   ```

   You should see pods for Prometheus and any associated components.

## Step 3: Expose Prometheus Service

1. **Expose the Prometheus service:**

   ```bash
   kubectl port-forward service/prometheus-server -n monitoring 9090:80
   ```

2. **Access Prometheus:**

   Open your browser and go to `http://localhost:9090` to verify that Prometheus is running.

## Step 4: Deploy Grafana

1. **Add the Grafana Helm repository:**

   ```bash
   helm repo add grafana https://grafana.github.io/helm-charts
   helm repo update
   ```

2. **Install Grafana using Helm:**

   ```bash
   helm install grafana grafana/grafana --namespace monitoring
   ```

3. **Verify the installation:**

   Check that the Grafana pod is running:

   ```bash
   kubectl get pods -n monitoring
   ```

4. **Expose the Grafana service:**

   ```bash
   kubectl port-forward service/grafana -n monitoring 3000:80
   ```

5. **Access Grafana:**

   Open your browser and go to `http://localhost:3000`. The default username is `admin`, and the password is also `admin`.

## Step 5: Add Prometheus as a Data Source in Grafana

1. **Login to Grafana.**
2. **Add Data Source:**
   - Navigate to **Configuration** (gear icon) > **Data Sources**.
   - Click **Add data source**.
   - Select **Prometheus** from the list.
3. **Configure the Prometheus data source:**
   - Set the URL to `http://prometheus-server.monitoring.svc.cluster.local:80` (this points to the Prometheus service in the cluster).
   - Click **Save & Test** to verify the connection.

## Step 6: Create Dashboards

Now that you have Prometheus set up as a data source, you can create dashboards using the metrics collected by Prometheus.

## Conclusion

You have successfully set up Prometheus as a data source for Grafana in a Minikube environment. You can now start monitoring your applications and creating visualizations in Grafana.

## Troubleshooting

If you encounter any issues during the setup, please check the following:

- Ensure that Minikube is running and accessible.
- Verify that the Prometheus and Grafana pods are running without errors.
- Check the logs for Prometheus and Grafana for any error messages.

For further assistance, consult the [Prometheus documentation](https://prometheus.io/docs/introduction/overview/) and [Grafana documentation](https://grafana.com/docs/grafana/latest/).
