# Services Lab

---

This document describes how to explore Services.

---

```console
# Show all the Services in the cluster
kubectl get svc --all-namespaces

# Show cluster IPs of Services in the Jobs NameSpace
kubectl -n jenkins get svc

# Show selectors, EndPoints and ports
kubectl -n jenkins describe svc jenkins
kubectl -n jenkins get ep jenkins

# Show Pod IP 
kubectl -n jenkins get pod -l app=jenkins -o wide

# Show Pod labels and ports
kubectl -n jenkins describe pod $(kubectl -n jenkins get pod -l app=jenkins -o=jsonpath="{.items[0].metadata.name}")

# Test DNS entry and see how it maps to the Service ClusterIP
kubectl run curl -it --rm --restart=Never --image appropriate/curl -- sh
> ping jenkins

# See DNS entry in another NameSpace
> ping kubernetes.default

# See the FQDN of a DNS entry in another NameSpace
> ping kubernetes.default.svc.cluster.local

# Show DNS resolution
> cat /etc/resolv.conf
```

Now that you are more familiar with Services:

* Find another Service.
* Check the selectors and port definitions of the Service.
* Find a Pod that is an EndPoint of the Service.
* Check the labels and ports of the Pod and see how they are referenced in the Service.
* Create a `curl` Pod and check the DNS resolves to the ClusterIP.