# Prometheus Helm

## Resources

* https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-prometheus-stack
* https://www.youtube.com/watch?v=6xmWr7p5TE0&t=68s

## Installation

```bash
choco install kubernetes-helm
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm show values prometheus-community/kube-prometheus-stack > values.yaml
helm install prometheus-helm prometheus-community/kube-prometheus-stack -f ./values.yaml
kubectl create -f .\node-ports.yaml
```

Visit Prometheus at `http://localhost:30000`. Visit Grafana at `http://localhost:30001` with `admin` and `prom-operator`.


## Resources

```bash
kubectl get all
NAME                                                         READY   STATUS
pod/alertmanager-prometheus-helm-kube-prome-alertmanager-0   2/2     Running
pod/prometheus-helm-grafana-89f94cd89-9f5xw                  3/3     Running
pod/prometheus-helm-kube-prome-operator-649d8cf85-brrxq      1/1     Running
pod/prometheus-helm-kube-state-metrics-59455d6586-9qbjn      1/1     Running
pod/prometheus-helm-prometheus-node-exporter-gr2sx           0/1     CrashLoopBackOff
pod/prometheus-prometheus-helm-kube-prome-prometheus-0       2/2     Running
pod/ubuntu                                                   1/1     Running

NAME                                               TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)
service/alertmanager-operated                      ClusterIP   None             <none>        9093/TCP,9094/TCP,9094/UDP
service/kubernetes                                 ClusterIP   10.96.0.1        <none>        443/TCP
service/prometheus-helm-grafana                    ClusterIP   10.102.214.152   <none>        80/TCP
service/prometheus-helm-kube-prome-alertmanager    ClusterIP   10.97.95.179     <none>        9093/TCP,8080/TCP
service/prometheus-helm-kube-state-metrics         ClusterIP   10.96.251.104    <none>        8080/TCP
service/prometheus-helm-prometheus-node-exporter   ClusterIP   10.97.62.185     <none>        9100/TCP
service/prometheus-operated                        ClusterIP   None             <none>        9090/TCP

NAME                                                      DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR
daemonset.apps/prometheus-helm-prometheus-node-exporter   1         1         0       1            0           kubernetes.io/os=linux

NAME                                                  READY   UP-TO-DATE   AVAILABLE
deployment.apps/prometheus-helm-grafana               1/1     1            1
deployment.apps/prometheus-helm-kube-prome-operator   1/1     1            1
deployment.apps/prometheus-helm-kube-state-metrics    1/1     1            1 

NAME                                                            DESIRED   CURRENT   READY
replicaset.apps/prometheus-helm-grafana-89f94cd89               1         1         1
replicaset.apps/prometheus-helm-kube-prome-operator-649d8cf85   1         1         1 
replicaset.apps/prometheus-helm-kube-state-metrics-59455d6586   1         1         1

NAME                                                                    READY
statefulset.apps/alertmanager-prometheus-helm-kube-prome-alertmanager   1/1
statefulset.apps/prometheus-prometheus-helm-kube-prome-prometheus       1/1
```

##  Pods
* `alertmanager-prometheus-helm-kube-prome-alertmanager-0`: Is a pod that runs AlertManager.
* `prometheus-helm-grafana` is a pod that runs Grafana.
* `prometheus-helm-kube-prome-operator`: Is a pod that observes the custom resource definitions and does the actual work.
* `kube-state-metrics`: Is a pod that runs once in the cluster and exports cluster statistics.
* `prometheus-node-exporter`: Is a pod running on each node that exports the node statistics like CPU, memory and network.
* `prometheus-prometheus-helm-kube-prome-prometheus`: Is a pod running Prometheus itself.


