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
## Design patterns

### SideCar
* Having a separate container for the log, like log-agent is a side car approach.
* The functionality of log-agent is to collect the logs from the other containers and send the logs to a Log Server.

### Adapater
* Let's say we have multiple applications generating logs in a different format, it would be hard to process various formats of the logs on the log server. 
* Before sending the logs to the log server, you would like to convert the logs to a common format.
* For this you deploy an adapter container
* The adapter container processes the logs before sending them to the log server

### Ambassador
* You application communicates with different databases depending on the environment, like Dev, Test, Prod.
* The application code has to be changed to point to correct db, before deploying the application to corresponding environment.
* If we choose to outsource such a configuration to separate container with in a pod, you application can always refer to a db at localhost.
* And the new container would proxy the request to the right container
