# Steps to Using Prometheus and Grafana in a GKE cluster
A Kubernetes Monitoring with Prometheus and Grafana Using GKE

## Task 1. Deploy GKE cluster
```bash
gcloud container clusters create gmp-cluster --num-nodes=3 --zone=us-west1-a
```
```bash
gcloud container clusters get-credentials gmp-cluster --zone=us-west1-a
```
```bash
kubectl create ns gmp-test
```
## Task 2. Deploy application
```bash
This example application emits Prometheus metrics on its metrics port. The application uses three replicas.
```bash
kubectl -n gmp-test apply -f https://raw.githubusercontent.com/GoogleCloudPlatform/prometheus-engine/v0.4.3-gke.0/examples/example-app.yaml
```
## Task 3. Deploy Prometheus

Run the following command to ingest a metric:

```bash
kubectl -n gmp-test apply -f https://raw.githubusercontent.com/GoogleCloudPlatform/prometheus-engine/v0.4.3-gke.0/examples/prometheus.yaml
```
```bash
kubectl -n gmp-test get pod
```

## Task 4. Prometheus metrics
```bash
export PROJECT_ID=$(gcloud config get-value project)
```
```bash
curl https://raw.githubusercontent.com/GoogleCloudPlatform/prometheus-engine/v0.4.3-gke.0/examples/frontend.yaml |
sed "s/\$PROJECT_ID/$PROJECT_ID/" | kubectl apply -n gmp-test -f -
```
```bash
kubectl -n gmp-test port-forward svc/frontend 9090
```
Task 5. Deploy Grafana
```bash
git clone https://github.com/prometheus-operator/kube-prometheus.git
```
```bash
cd kube-prometheus
```
```bash
kubectl -n gmp-test apply -f https://raw.githubusercontent.com/GoogleCloudPlatform/prometheus-engine/v0.4.3-gke.0/examples/grafana.yaml
```
## Task 6. Grafana
- Log in to Grafana using the username admin and password admin.

- Click Skip when asked to enter new password.

## Task 7. Configure data source
- To query Managed Service for Prometheus in Grafana by using the Prometheus UI as the authentication proxy, you must add new data source to Grafana. To add a data source for the managed service, do the following:

- Go to your Grafana deployment, for example, by browsing to the URL http://localhost:3000 to reach the Grafana welcome page.

- Select Configuration from the main Grafana menu, then select Data Sources.
  -  In the URL field of the HTTP pane, enter the URL of the Managed Service for Prometheus frontend service.
  -  If you configured the Prometheus UI to run on port 9090, then the service URL for this field is http://frontend.gmp-test.svc:9090.

- In the HTTP Method field, select GET.

## Task 8. Grafana chart

- You can now create Grafana dashboards using the new data source. You can also redirect existing dashboards to the new data source. 
- The following screenshot shows a Grafana chart that displays the up metric.

