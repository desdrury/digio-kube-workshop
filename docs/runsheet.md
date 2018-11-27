# digio-kube-workshop

## Nginx Ingress Controller

**Cluster wide (Metrics)**

```console
helm upgrade --install metrics-nginx-ingress \
  --namespace metrics \
  --set controller.scope.enabled=true \
  --set controller.scope.namespace=metrics \
  --version 0.22.1 \
  stable/nginx-ingress
```

**Cluster wide (Jenkins)**

```console
helm upgrade --install jenkins-nginx-ingress \
  --namespace jenkins \
  --set controller.scope.enabled=true \
  --set controller.scope.namespace=jenkins \
  --version 0.22.1 \
  stable/nginx-ingress
```


**Workshop participant**

Notes 

* Replace `<name>` with the participants names.
* The `values.yaml` must have a correct value for `namespace`.

```console
helm upgrade --install <name>-nginx-ingress \
  --namespace <name> \
  -f charts-values/nginx-ingress/values.yaml \
  --version 0.22.1 \
  stable/nginx-ingress
```


## Cert Manager

[Chart](https://github.com/kubernetes/charts/tree/master/stable/cert-manager)

```console
helm upgrade --install cert-manager \
  --namespace ingress \
  --set ingressShim.defaultIssuerName=letsencrypt-prod \
  --set ingressShim.defaultIssuerKind=ClusterIssuer \
  --version v0.4.1 \
  stable/cert-manager

kubectl -n ingress create -f charts-values/cert-manager/cluster-issuer.yaml
```


## Prometheus

**Cluster wide (Metrics)**

```console
helm upgrade --install metrics-prometheus \
  --namespace metrics \
  -f charts-values/prometheus/values.yaml \
  --version 6.10.0 \
  stable/prometheus
```

**Workshop participant**

**Notes**

* Replace `<name>` with the participants names.
* The `digio-values.yaml` must have a correct value for `hosts`.

```console
helm upgrade --install <name>-prometheus \
  --namespace <name> \
  -f charts-values/prometheus/digio-values.yaml \
  --version 6.10.0 \
  stable/prometheus
```


## Grafana

**Cluster wide (Metrics)**

```console
helm upgrade --install metrics-grafana \
  --namespace metrics \
  -f charts-values/grafana/values.yaml \
  --version 1.12.0 \
  stable/grafana
```

**Workshop participant**

**Notes**

* Replace `<name>` with the participants names.
* The `values.yaml` must have a correct value for `hosts`.
* The `values.yaml` must have a correct value for `url` so that the Prometheus datasource is found.

```console
helm upgrade --install <name>-grafana \
  --namespace <name> \
  -f charts-values/grafana/values.yaml \
  --version 1.12.0 \
  stable/grafana
```

## Jenkins

**Cluster wide (Jenkins)**

```console
helm upgrade --install jenkins-jenkins \
  --namespace jenkins \
  -f charts-values/jenkins/values.yaml \
  --version 0.16.18 \
  stable/jenkins
```

**Workshop participant**

**Notes**

* Replace `<name>` with the participants names.
* The `values.yaml` must have a correct value for `HostName`.

```console
helm upgrade --install <name>-jenkins \
  --namespace <name> \
  -f charts-values/jenkins/values.yaml \
  --version 0.16.18 \
  stable/jenkins
```


## DinD

**Cluster wide (Jenkins)**

```console
helm upgrade --install dind \
  --namespace jenkins \
  charts/dind/
```

**Workshop participant**

**Notes**

* Replace `<name>` with the participants names.

```console
helm upgrade --install dind \
  --namespace <name> \
  charts/dind/
```