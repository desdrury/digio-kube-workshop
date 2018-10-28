# Helm Lab

---

This document describes how to author a Helm Chart for the first-app built earlier.

---

Create template Helm Chart for `first-app`.

```console
# Create temporary directory if it does not already exist
mkdir ~/tmp
cd ~/tmp

# Create template Helm Chart
helm create first-app

# Open Helm Chart with your code editor (this uses VS Code)
code first-app
```

Explore the files in the Helm Chart directory.

Edit the `values.yaml` to use the Docker Hub `first-app` image you pushed in the earlier lab, i.e.

```yaml
...
image:
  repository: nginx
  tag: stable
...
```

Enable the `ingress` and update the `hosts` field.  No example is provided as it is an exercise to work out what needs to be done.

Test deploy the Helm Chart to see what resources will be created in the cluster.

```console
helm upgrade --install <name>-first-app \
  --namespace <name> \
  ~/tmp/first-app --debug --dry-run
```

Now deploy the Helm Chart.

```console
helm upgrade --install <name>-first-app \
  --namespace <name> \
  ~/tmp/first-app
```

Check the resources, i.e. `Pod`, `Service` and `Ingress`, using `kubectl`.

Check you can access `first-app` through the Ingress Controller.

