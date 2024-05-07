# Observability

## Readiness Probe
* Every application has some warm up period, before it is ready to accept traffic. 
* If the traffic is allowd before this warm up period, the user is going to face the issues
* So, it necessary to validate the pod, if it's ready, to allow the traffic
* Without ReadinessProbes, the application `ready` status changes immediently.
* So we can write a custom logic to check if the pod is ready, and only then change the status, which allows the traffic the application.

```yaml
apiVersion: v1
kind: Pod
metadata:
    name: simple-webapp
spec:
    containers:
    - name: simple-webapp
      image: simple-webapp
      ports:
      - containerPort: 8080
      readinessProbe:
        httpGet:
            path: /api/ready
            port: 8080
        initialDelaySeconds: 10
        periodSeconds: 5
        failureThreshold: 8
```

```yaml
      readinessProbe:
        tcpSocket:
            port: 3306
```

```yaml
      readinessProbe:
        exec:
            command:
                - cat
                - /app/is_ready
```
