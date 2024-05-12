# Pod Design

## Labels, Selectors

You can define the labels for the kuberentes resources in multiple ways.

Labels are defined on the resources of the kuberentes and will be helpful in filterig or sorting the resources based on the labels.

```bash
kubectl label deploy nginx app=web
```

You can add it in the metadata section of the resourse in the yaml file while creating or updating the resource.

```yaml
metadata:
    name: nginx
    lables:
        app: web
```

Once you define labels for the resource, you can use commands to filter them while searching. 

```bash
kubectl get deploy -l app=web
kubectl get deploy --selector app=web
```

## Annotations

Annotations are used to record the details for informatory purpose. For ex: tool details like name, version.

```yaml
metadata:
    name: simple-webapp
    annotations: 
        buildversion: 1.34
```
## Rolling updates and Rollbacks in deployment

### Rolling updates

When you update something in the deployment, then the deployment will start the rolling update the pods, to make the changes effective. 

You can check the status of the rollout using command

```bash
kubectl rollout status deploy <deployment-name>
```

To get the history of the deployment, you can run the command

```bash
kubctl rollout history deploy <deployment-name>
```

### Deployment Strategy

There are two types of deployment strategies
    1. Recreate
    2. **RollingUpdate** *Default*


