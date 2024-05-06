# Multi Container Pods

There are 3 different kinds of patterns
1. Ambassador
2. Adapter
3. Sidecar

## Why do we need them?

If you need application separated as microservice, each service doing a different job. And if the application life cycle should be the same, then you can go with multi container approach. 

Here you can have two or more containers in a single pod, doing separate funtionalities but having the same pod lifecycle. You can scale them up and down together.

* They share the same network space and they can communicate between each other using `localhost`
* They have access to the same storage volumes, this way you do not have to establish volume sharing or services between the pods to enable communication between them.

```yaml
apiVersion: v1
kind: Pod
metadata:
    name: simple-webapp
    labels:
        name: simple-webapp
spec:
    containers:
    - name: simple-webapp
      image: simple-webapp
      ports:
      - containerPort: 8080
    - name: log-agent
      image: log-agent
```


