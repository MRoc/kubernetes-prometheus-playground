# Prometheus Operator

## Install using Kube-Prometheus

Deploy Prometheus on Kubernetes following https://prometheus-operator.dev/ and Following https://prometheus-operator.dev/docs/getting-started/installation/.

```bash
git clone https://github.com/prometheus-operator/kube-prometheus.git
cd kube-prometheus
kubectl create -f manifests/setup
kubectl create -f manifests/
```

```bash
cd ..
cd t2-operator
kubectl create -f ./sample-app.yaml
http://localhost:30010/

kubectl create -f ./service-monitor.yaml
kubectl create -f ./service-account.yaml
kubectl create -f ./cluster-role.yaml
kubectl create -f ./cluster-role-binding.yaml
kubectl create -f ./prometheus.yaml
kubectl create -f ./prometheus-node-port.yaml
```

## Problems

This did not work for me. Prometheus and Grafana were not accessible. Problems:

- I can not access Prometheus or Grafana through NodePort.
- I can not access pods through ClusterIP.
- I can access both Prometheus pods directly using `wget http://<pod-ip-1>:9090` and `wget <pod-ip-2>:9090` from ubuntu [1] inside cluster.
- Inspecting ClusterIP using `kubectl describe svc prometheus-k8s -n monitoring` shows that it is bound to endpoints `<pod-ip-1>:9090,<pod-ip-2>:9090` which are IPs of pods.
- I can not access Grafana through ClusterIP `wget http://<cluster-ip>:3000` but directly through pod using `wget http://<pod-ip>:3000`.
- Can I access the sample app through pod, clusterID and node port? `kubectl create -f .\sample-app.yaml`. Then `wget <sample-ip-ip>:8080` from ubuntu to access pod works fine.

--- 

[1] `kubectl run ubuntu -n monitoring --image=ubuntu -it`