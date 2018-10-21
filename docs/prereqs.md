# Prereqs

---

This document describes the tools that need to be installed prior to the workshop.  It then goes on to describe how to create a _Minikube_ cluster and install the _Add-Ons_.

**Note**

These instructions expect that OSX will be used.

---


## Tools

The tools we will install are:

| Tool      | Description               |
|:----------|:--------------------------|
| [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/) | The Kubernetes CLI. |
| [Virtual Box](https://www.virtualbox.org) | The virtual machine hypervisor used by `minikube`. |
| [Minikube](https://github.com/kubernetes/minikube) | A single-node Kubernetes cluster inside a VM. |
| [Helm](https://helm.sh) | The Kubernetes package manager. |
| [Visual Studio Code](https://code.visualstudio.com) | A code editor. *OPTIONAL*. |
| [kubectx and kubens](https://github.com/ahmetb/kubectx) | Helper scripts for changing Kubernetes contexts and namespaces. *OPTIONAL*. |
| [kubetail](https://github.com/johanhaleby/kubetail) | Helper script to easily tail logs from Pods. *OPTIONAL*. |

**Notes**

* `kubectx`, `kubens` and `kubetail` only work on Mac or Linux systems.
* A Mac has been used for these instructions.  However the commands for Linux will be very similar, if not the same.  And Windows will also be somewhat similar.


### Kubectl

The instructions for installing `kubectl` can be found [here](https://kubernetes.io/docs/tasks/tools/install-kubectl/).  

There are various ways to install `kubectl` but we will focus on how to do it using `curl`.  Scroll down the page until you reach the section _Install kubectl binary via curl_.  We will install the latest version of `kubectl`, so use the following commands.

```console
curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/darwin/amd64/kubectl

chmod +x ./kubectl

sudo mv ./kubectl /usr/local/bin/kubectl
```


### Virtual Box

The installation packages for _Virtual Box_ can be found [here](http://www.oracle.com/technetwork/server-storage/virtualbox/downloads/index.html).

For Mac, click on the link for the _dmg Image_.

Once the package has finished downloading, open it and follow the prompts.


### Minikube

The releases for `minikube` can be found [here](https://github.com/kubernetes/minikube/releases).

We will be installing version `v0.27.0`.  Use the following commands.

```console
curl -Lo minikube https://storage.googleapis.com/minikube/releases/v0.27.0/minikube-darwin-amd64 \
  && chmod +x minikube \
  && sudo mv minikube /usr/local/bin/
```

### Helm

The releases for `helm` can be found [here](https://github.com/kubernetes/helm/releases).

We will be using version `v2.9.1`.  Use the following commands.

```console
curl -L https://storage.googleapis.com/kubernetes-helm/helm-v2.9.1-darwin-amd64.tar.gz | tar xzvf - \
  && sudo mv darwin-amd64/helm /usr/local/bin/ \
  && rm -Rf darwin-amd64
```


### Visual Studio Code (optional)

The installation packages for _Visual Studio Code_ can be found [here](https://code.visualstudio.com).

Click on the _Download for_ button.

Once the package has finished downloading, move it into the applications directory using the Mac Finder.


### Kubectx and Kubens (optional)

The instructions for installing `kubectx` and `kubens` can be found [here](https://github.com/ahmetb/kubectx).  

For the Mac, the instructions describe using the _Homebrew_ Mac package manager.  If you do not already have _Homebrew_ installed you can find its installation instructions [here](https://brew.sh).

Once you have _Homebrew_ installed use the following commands.

```console
brew install kubectx --with-short-names
```

**Note**

* Using the `--with-short-names` argument to `brew` will install the commands with their short names.
    * `kubectx` will become `kctx`.
    * `kubens` will become `kns`.


### Kubetail (optional)

The instructions for installing `kubetail` can be found [here](https://github.com/johanhaleby/kubetail).

Once again _Homebrew_ will be used for the installation.  Use the following commands.

```console
brew tap johanhaleby/kubetail && brew install kubetail
```

---

## Validate

Once the tools have been installed you need to create a cluster using _Minikube_.

The Git repo for _Minikube_ can be found [here](https://github.com/kubernetes/minikube).  It provides some great documentation.  With the releases available [here](https://github.com/kubernetes/minikube/releases).

The first thing we want to do is see what versions of _Kubernetes_ are available.

```console
minikube get-k8s-versions
```

You can create a cluster with just `minikube start` but we want to customise the configuration a bit.  So let's have a look at the possible options.

```console
minikube start -h
```

For our first cluster we want to use Kubernetes version `v1.10.0` and give it 2 cpus with 4GB memory.  Note that by default [`kubeadm`](https://kubernetes.io/docs/setup/independent/create-cluster-kubeadm/) will be used to bootstrap the cluster.  `kubeadm` is now the standard way to create a Kubernetes cluster and is used by many different Kubernetes distributions.

```console
minikube start --cpus 2 --memory 4096 --kubernetes-version v1.10.0 --insecure-registry "10.0.0.0/24"
```

**Notes**

* Sometimes the default `kubeadm` bootstrapper does not work.  If this is the case then delete the cluster using `minikube delete` and then add the `-b localkube` flag to the end of the previous command.
* The `--insecure-registry` flag is used so that you can work with the _Docker Registry_ Add-On.

The first time a cluster is created it will take a bit of time as the ISO for the VM and the Docker containers for the _Kubernetes_ control plane are downloaded.  These items will be cached in `~/.minikube` so any future clusters will be created faster (if using the same _Kubernetes_ version).

Once our cluster is created we want to check on its status.

```console
minikube status
```

Although having a _Kubernetes_ cluster is great, it's not entirely useful yet.  _Minikube_ provides a set of Add-Ons for some of the more common needs in a _Kubernetes_ cluster.  

Lets see what Add-Ons are available.

```console
minikube addons list
```

As you can see, some of the Add-Ons are enabled by default.  We want to add a few more.

```console
minikube addons enable ingress
minikube addons enable registry
minikube addons enable heapster
minikube addons enable dashboard
```

After a while the Docker containers for the Add-Ons should be running.  We can then access the _Kubernetes Dashboard_.

```console
minikube addons open dashboard
```

or.

```console
minikube dashboard
```

We can also see what services are available in the cluster.

```console
minikube service list
```

And the services give us yet another way to access the dashboard.

```console
minikube service kubernetes-dashboard -n kube-system
```

One of the Add-Ons we added provides some basic cluster metrics.

```console
minikube addons open heapster
```

If we want to see the _Minikube_ logs we can also do that.

```console
minikube logs
```

Another great feature of _Minikube_ is that it can be used as our local _Docker_ daemon.

```console
eval `minikube docker-env`
docker ps
```

And if you need additional tools to debug things on the cluster you can use the [CoreOS Toolbox](https://github.com/coreos/toolbox).

```console
minikube ssh toolbox
```