# Taints and Tolerations

* Taints are set on nodes
* Tolerations are set on the pods

This whole mechanisam is to limit pods getting assigned to a particular node. 

For Ex: if you want to assign only specific pods to a node and every other pod should be assigned to remaning nodes, then you Taint the node and it does not allow any pod inside it. 

Now for the pod for which you want to place in the node, you add tolerence to the pod which will be above the rule specified for teh node and will be assigned to that particular node. 

## Taint - Nodes

```bash
kubectl taint nodes node-name key=value:taint-effect
```
In order to remove the taint or untaint

```bash
kubectl taint node node-name key=value:taint-effect-
```
The last `-` is going to untaint the node.

### taint-effect

These are for the pods which are not tolerent
    1. NoSchedule - pods will not scheduled on this node
    2. PreferNoSchedule - pods will not scheduled, but there would not be any guarenetee for that
    3. NoExecutre - New pods will not be scheduled on the node and existing pods will be evicted if they do not tolerate the taint

```bash
kubectl taint nodes node1 app=blue:NoSchedule
```

## Tolearations - Pod

[pod-definition](pod-definition.yaml)

## Master node

Earlier discussion happened on worker nodes

what about master?

The master node by default has a taint, to ristrict the pods. That's why the pods are assigned to worker nodes. 

You can check that by running command.

```bash
kubectl describe nod kubemaster | grep Taint
```
