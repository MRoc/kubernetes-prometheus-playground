# Prometheus Manual

Manually deploy Prometheus on Kubernetes using YAML and ConfigMaps following https://devopscube.com/setup-prometheus-monitoring-on-kubernetes/. No custom resource definitions (CRD) are used.

```bash
kubectl config use-context docker-desktop
kubectl config current-context
kubectl get pods --all-namespaces
```

```bash
kubectl apply -f ./namespace.yaml
kubectl apply -f ./cluster-role.yaml
kubectl apply -f ./config-map.yaml
kubectl apply -f ./prometheus-deployment.yaml
kubectl apply -f ./prometheus-service.yaml
kubectl get all -n monitoring
```

Visit http://localhost:30000/