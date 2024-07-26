# Prometheus Helm

https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-prometheus-stack

```bash
choco install kubernetes-helm
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm install prometheus-helm prometheus-community/kube-prometheus-stack
kubectl create -f .\node-ports.yaml
```

Visit Prometheus at `http://localhost:30000`.

Visit Grafana at `http://localhost:30001` with `admin` and `prom-operator`.