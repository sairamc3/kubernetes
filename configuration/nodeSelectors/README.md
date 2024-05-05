# Node selectors

* Label the nodes with some key value pair
    Ex: size:Large

```bash
kubectl label nodes <node-name> <label-key>=<label-value>
kubectl label nodes node01 size=Large
```
* If you want to assign a pod to a particular node, you can use lables in the `nodeSelector` field of the pod. 

Ref: [pod-definition](pod-definition.yaml)

Although it looks like a good feature, there are some shortcomings. 

What if you have a complex requirement. 

Ex: If you want to assign a pod to any node which is not `small`, like `Large` or `Medium`, you cannot acheive it using Node selectors.

That is where [Node Affinity](../NodeAffinity) comes into picture. 
