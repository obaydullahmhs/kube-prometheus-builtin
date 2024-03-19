# kube-prometheus-builtin

# Apply necessary resources
```bash
kubectl create -f grafana.yaml -f kube-state-metrics.yaml 
kubectl create -f prom-configmap.yaml -f prom-rbac.yaml -f prom-svc.yaml -f prom-deployment.yaml
```

# Port-forward the following services
```bash
kubectl port-forward -n monitoring service/grafana 3000:80
kubectl port-forward -n monitoring service/prometheus 9090
```

Create api key from grafana `http://localhost:3000/org/apikeys` and add in `grafana-connectcluster.yaml and grafana-kafka.yaml` and modify resource `name` and `namespace`


```helm
helm upgrade -i panopticon appscode/panopticon -n kubeops --create-namespace --version=v2024.2.5 \
                             --create-namespace \
                             --set monitoring.enabled=true \
                             --set monitoring.agent=prometheus.io/builtin \
                             --set-file license=kubedb-license-file.txt
```

# Add grafana dashboard for kafka and connect cluster
```helm
helm upgrade -i  kubedb-grafana-dashboards $HOME/go/src/kubedb.dev/installer/charts/kubedb-grafana-dashboards/ -n kubeops --create-namespace --version=v2024.2.14 --values ./grafana-connectcluster.yaml
helm upgrade -i  kubedb-grafana-dashboards $HOME/go/src/kubedb.dev/installer/charts/kubedb-grafana-dashboards/ -n kubeops --create-namespace --version=v2024.2.14 --values ./grafana-kafka.yaml
```
