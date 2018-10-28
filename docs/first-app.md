# First Application

---

This document describes how to build and test an application.

---

Build application.

```console
cd first-app
minikube `docker-env`
docker build -t first-app:v1 .
```

Test application with _Docker_.

```console
docker run -d -p 8000:80 first-app:v1
curl `minikube ip`:8000
```

Test application with _Minikube_.

```console
kubectl -n default apply -f kubernetes
curl `minikube service -n default first-app --url`
```

To use the application later you will need to push it to Docker Hub.

```
docker login
docker tag first-app:v1 <docker-hub-account>/first-app:v1
docker push
```
