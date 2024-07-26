# prometheus-operator

Deploy Prometheus on Kubernetes following https://prometheus-operator.dev/ and Following https://prometheus-operator.dev/docs/getting-started/installation/.

## Install using Kube-Prometheus

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

Failed. Did not work for me. Also the tutorial was very unprecise with the steps required.