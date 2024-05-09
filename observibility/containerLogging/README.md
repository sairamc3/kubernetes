# Container Logging

In order to stream the logs of a container, in a single container pod

```bash
kubectl logs -f <pod-name>
```
for a multicontainer pod, you need to specify the container name as well.

```bash
kubectl logs -f <pod-name> <container-name>
```
