# Node Affinity

By using node affinity we have a lot of flexibility in selecting the nodes for our pods. 

The format would look much complex though. 

[pod-definition](pod-definition.yaml)

In the match Expression field you can also use something like:

```yaml
- matchExpressions:
    - key: size
      operator: NotIn
      values:
      - Small
```

Just checks if the label `size` exists in the node
```yaml
- matchExpressions:
    - key: size
      operator: Exists
```

## Type of Node Affinity

### Available 
`requiredDuringSchedulingIgnoredDuringExecution`

`preferredDuringSchedulingIgnoredDuringExecution`

### Planned

`requiredDuringSchedulingRequiredDuringExecution`

**DuringScheduling** is a phase where the pod does not exists and it is created for the first time. 
**DuringExecution** is the phase where the pod exists in the node and the node configuration is changed, like removing the labels.

||DuringScheduling|DuringExecution|
|-|---------------|---------------|
|Type-1| Required | Ignored|
|Type-2| Preferred | Ignored|
|Type-3| Required | Required|

## TaintsTolerence Vs Node affinity

Both has their shortcomings, combining them together will give us the required functionalities.

### ShortComings

#### Taints
* When you taint a node, it will not allow other pods into its nodes. 
* The pods with Tolerence can go into the nodes.
* The pods with Tolerence can also go into other nodes, since there are no taints for the other nodes.
* Hence this will not make sure that the pods will end up in the desired nodes

#### Node Affinity
* With Node Affinity, You can point a pod to a particular node using labels.
* But that particular node will not restrict other pods into the nodes.
* In this case the nodes will have both the configured pods and unconfigured pods in the node

### Combining both
* When you use both of them, by tainting the nodes, restricting the unTolerated pods to the node.
* Using Node Affinity to point the pods to the node.
* So the pods will the desired node and the desired node will not have pods other than the tolerated pods.
