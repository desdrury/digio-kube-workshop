# Workloads

---

This document describes how a workshop attendee can deploy workloads to the cluster.

---

## Nginx Ingress Controller

[Chart](https://github.com/helm/charts/tree/master/stable/nginx-ingress)

**Notes**

* Replace `<name>` with your name.

```console
helm upgrade --install <name>-nginx-ingress \
  --namespace <name> \
  --set controller.scope.enabled=true \
  --set controller.scope.namespace=<name> \
  --version 0.22.1 \
  stable/nginx-ingress
```

Creating an Ingress Controller will cause GCE to provision an external LoadBalancer.  You can find the IP addess of the external LoadBalancer as below.

```console
kubectl -n <name> get svc
NAME                                    TYPE           CLUSTER-IP      EXTERNAL-IP     PORT(S)                      AGE
metrics-nginx-ingress-controller        LoadBalancer   10.19.247.254   35.197.171.23   80:30389/TCP,443:30740/TCP   3m
metrics-nginx-ingress-default-backend   ClusterIP      10.19.240.2     <none>          80/TCP                       3m
```

A DNS A record then needs to be setup for the IP address.


## Prometheus

[Chart](https://github.com/helm/charts/tree/master/stable/prometheus)

**Notes**

* Replace `<name>` with your name.
* The `digio-values.yaml` must have a correct value for `hosts`.

```console
helm upgrade --install <name>-prometheus \
  --namespace <name> \
  -f charts-values/prometheus/digio-values.yaml \
  --version 6.10.0 \
  stable/prometheus
```


## Grafana

[Chart](https://github.com/helm/charts/tree/master/stable/grafana)

**Notes**

* Replace `<name>` with your name.
* The `digio-values.yaml` must have a correct value for `hosts`.
* The `digio-values.yaml` must have a correct value for `url` so that the Prometheus datasource is found.

```console
helm upgrade --install <name>-grafana \
  --namespace <name> \
  -f charts-values/grafana/digio-values.yaml \
  --version 1.12.0 \
  stable/grafana
```

Get the `admin` password with the following command.

```
kubectl get secret --namespace <name> <name>-grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
```

Then the dashboards must be imported from the `grafana-dashboards/` directory.


## Jenkins

[Chart](https://github.com/helm/charts/tree/master/stable/jenkins)

**Notes**

* Replace `<name>` with your name.
* The `digio-values.yaml` must have a correct value for `HostName`.

```console
helm upgrade --install <name>-jenkins \
  --namespace <name> \
  -f charts-values/jenkins/digio-values.yaml \
  --version 0.16.18 \
  stable/jenkins
```

Get the `admin` password with the following command.

```
printf $(kubectl get secret --namespace <name> <name>-jenkins -o jsonpath="{.data.jenkins-admin-password}" | base64 --decode);echo
```


## DinD

**Notes**

* Replace `<name>` with your name.

```console
helm upgrade --install dind \
  --namespace <name> \
  charts/dind/
```