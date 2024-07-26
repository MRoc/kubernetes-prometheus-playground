# Prometheus Helm

## Resources

* https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-prometheus-stack
* https://www.youtube.com/watch?v=6xmWr7p5TE0&t=68s

## 1. Installation

```bash
choco install kubernetes-helm
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm show values prometheus-community/kube-prometheus-stack > values.yaml
helm install prometheus-helm prometheus-community/kube-prometheus-stack -f ./values.yaml
```

## 2. Connectivity

### Node port

```bash
kubectl create -f .\prometheus-node-ports.yaml
```

Visit Prometheus at `http://localhost:30000`. Visit Grafana at `http://localhost:30001` with `admin` and `prom-operator`.

### Port forward

```bash
kubectl port-forward prometheus-prometheus-helm-kube-prome-prometheus-0 9090
```

Visit `http://localhost:9090`.


## 3. Example app

```bash
kubectl create -f .\example-app.yaml
```

The YAML creates an example app, a cluster IP, a node port and a *Service Monitor*. Using a *Service Monitor*, we can define a set of targets for *Prometheus* to monitor and scrape. It is a declarative way to define prometheus configs.

Visit `http://localhost:30002` to see the example app. Visit `http://localhost:30000/targets` to see if the example app is listed inside *Prometheus*.


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

```bash
kubectl get secrets
alertmanager-prometheus-helm-kube-prome-alertmanager                Opaque               1      17m
alertmanager-prometheus-helm-kube-prome-alertmanager-generated      Opaque               1      17m
alertmanager-prometheus-helm-kube-prome-alertmanager-tls-assets-0   Opaque               0      17m
alertmanager-prometheus-helm-kube-prome-alertmanager-web-config     Opaque               1      17m
prometheus-helm-grafana                                             Opaque               3      17m
prometheus-helm-kube-prome-admission                                Opaque               3      163m
prometheus-prometheus-helm-kube-prome-prometheus                    Opaque               1      17m
prometheus-prometheus-helm-kube-prome-prometheus-tls-assets-0       Opaque               1      17m
prometheus-prometheus-helm-kube-prome-prometheus-web-config         Opaque               1      17m
sh.helm.release.v1.prometheus-helm.v1                               helm.sh/release.v1   1      17m
```

```bash
kubectl get cm
prometheus-helm-grafana                                        1      17m
prometheus-helm-grafana-config-dashboards                      1      17m
prometheus-helm-kube-prome-alertmanager-overview               1      17m
prometheus-helm-kube-prome-apiserver                           1      17m
prometheus-helm-kube-prome-cluster-total                       1      17m
prometheus-helm-kube-prome-controller-manager                  1      17m
prometheus-helm-kube-prome-etcd                                1      17m
prometheus-helm-kube-prome-grafana-datasource                  1      17m
prometheus-helm-kube-prome-grafana-overview                    1      17m
prometheus-helm-kube-prome-k8s-coredns                         1      17m
prometheus-helm-kube-prome-k8s-resources-cluster               1      17m
prometheus-helm-kube-prome-k8s-resources-multicluster          1      17m
prometheus-helm-kube-prome-k8s-resources-namespace             1      17m
prometheus-helm-kube-prome-k8s-resources-node                  1      17m
prometheus-helm-kube-prome-k8s-resources-pod                   1      17m
prometheus-helm-kube-prome-k8s-resources-workload              1      17m
prometheus-helm-kube-prome-k8s-resources-workloads-namespace   1      17m
prometheus-helm-kube-prome-kubelet                             1      17m
prometheus-helm-kube-prome-namespace-by-pod                    1      17m
prometheus-helm-kube-prome-namespace-by-workload               1      17m
prometheus-helm-kube-prome-node-cluster-rsrc-use               1      17m
prometheus-helm-kube-prome-node-rsrc-use                       1      17m
prometheus-helm-kube-prome-nodes                               1      17m
prometheus-helm-kube-prome-nodes-darwin                        1      17m
prometheus-helm-kube-prome-persistentvolumesusage              1      17m
prometheus-helm-kube-prome-pod-total                           1      17m
prometheus-helm-kube-prome-prometheus                          1      17m
prometheus-helm-kube-prome-proxy                               1      17m
prometheus-helm-kube-prome-scheduler                           1      17m
prometheus-helm-kube-prome-workload-total                      1      17m
prometheus-prometheus-helm-kube-prome-prometheus-rulefiles-0   35     17m
```

```bash
kubectl get crd
NAME                                        CREATED AT
alertmanagerconfigs.monitoring.coreos.com   2024-07-26T08:55:38Z
alertmanagers.monitoring.coreos.com         2024-07-26T08:55:38Z
podmonitors.monitoring.coreos.com           2024-07-26T08:55:38Z
probes.monitoring.coreos.com                2024-07-26T08:55:38Z
prometheusagents.monitoring.coreos.com      2024-07-26T08:55:38Z
prometheuses.monitoring.coreos.com          2024-07-26T08:55:38Z
prometheusrules.monitoring.coreos.com       2024-07-26T08:55:38Z
scrapeconfigs.monitoring.coreos.com         2024-07-26T08:55:38Z
servicemonitors.monitoring.coreos.com       2024-07-26T08:55:38Z
thanosrulers.monitoring.coreos.com          2024-07-26T08:55:39Z
```

##  Pods
* `alertmanager-prometheus-helm-kube-prome-alertmanager-0`: Is a pod that runs AlertManager.
* `prometheus-helm-grafana` is a pod that runs Grafana.
* `prometheus-helm-kube-prome-operator`: Is a pod that observes the custom resource definitions and does the actual work.
* `kube-state-metrics`: Is a pod that runs once in the cluster and exports cluster statistics.
* `prometheus-node-exporter`: Is a pod running on each node that exports the node statistics like CPU, memory and network.
* `prometheus-prometheus-helm-kube-prome-prometheus`: Is a pod running **Prometheus itself**.

## Configuration

https://prometheus.io/docs/prometheus/latest/configuration/configuration/