# API Lab

---

This document describes how to explore the Kubernetes API.

---


```console
# Get the API groups and versions for the cluster
kubectl api-versions

# Get the API resources for the cluster
kubectl api-resources

# Get the API resources for the extensions API group
kubectl api-resources --api-group=extensions

# Explain a Deployment API resource from the apps/v1 API group
kubectl explain deployment --api-version apps/v1
kubectl explain deployment.spec.replicas --api-version apps/v1
kubectl explain deployment.spec --api-version apps/v1 --recursive
```

Now that you are more familiar with API resources go ahead and explore a number of them.