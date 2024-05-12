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

**Recreate**: It will take down all the current pods and scale up new pods. So, there would be downtime during this process.

**RollingUpdate**: It will scale down few pods and scale up new pods. Once the new pods are up, only then it will proceed with scaling down the previous ones. If there is any issue in between, it would stop the process and clients stay unimpacted.

## BLUE-GREEN deployment vs Canary Deployment

How both can be achieved in kuberetes

### Blue-Green Deployment

It is the strategy in which you have a version of the application and then you deploy a new version. The traffic only goes to the old version. Now test the new version of the application. If everything is fine then route the traffic at once to the new version. 

The traffic routing part is controlled by `service`. It is connected to the `deployment` or `pod` by the matching lables. 

For Ex:

You have a deployment with lable `version: v1` and service with `selector: matchLabels: version: v1`. Now you deploy a new version with label `version: v2`. You can test the application and if everything is fine then you can change the service to `selector: matchLabels: version: v2`, which would route the traffic to the new version. 

### Canary Deployment

It is the strategy in which you would gradually route the traffic to the new version. You have a version of the application and you deploy a new version. Both the versions would receive the traffic. You can scale up the new version to recieve the traffic and scale down the previous version. 

In k8, you should have a common lable between both the versions. something like `app: front-end`.

Now map service to the common lable. Both the versions would be eligible to recieve the traffic and the amount of the traffic is controlled by the number of pods in each of the versions. 
