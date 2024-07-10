# This repo shows how we can use prometheus and grafana for monitoring our kubernetes resources

first you should have a kubernetes cluster to work with i created a AKS cluster

# Step 1: Set up Helm

curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash

# Step 2: Add Helm Repositories

helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update

# Step 3: Install Prometheus

helm install prometheus prometheus-community/prometheus \
  --namespace monitoring --create-namespace
  
# Step 4: Install Grafana

helm repo add grafana https://grafana.github.io/helm-charts
helm repo update
helm install grafana grafana/grafana --namespace monitoring --create-namespace


# Step 5: check Prometheus & Grafana pods are started
kubectl get pods --namespace monitoring

# Step 6: Get Grafana admin password

kubectl get secret --namespace monitoring grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo

# Step 7: Port forward to access Grafana

kubectl port-forward --namespace monitoring svc/grafana 3000:80

# Step 8: Add Prometheus as a Data Source in Grafana
In Grafana, go to Configuration (the gear icon) > Data Sources.
Click Add data source.
Select Prometheus from the list.
In the HTTP section, set the URL to the Prometheus server. If Prometheus is installed in the monitoring namespace, the URL should be:

http://prometheus-server.monitoring.svc.cluster.local

Scroll down and click Save & Test to verify the connection.

# Step 9: Create Dashboards
After successfully adding Prometheus as a data source, go to Dashboards > Manage.
Click New Dashboard.
Add a new panel and select Prometheus as the data source.
Enter a Prometheus query to visualize the data. For example, you can use a query like up to show the status of your targets.
Now you have Prometheus metrics available in Grafana, and you can start creating dashboards to visualize the metrics.


visualization of memory usage with namespace monitoring in grafana

![image](https://github.com/sathishkumar-2001/k8s_monitoring/assets/126504329/4386537f-b6e1-4f6b-b3cf-9bcb1c78f2d9)

