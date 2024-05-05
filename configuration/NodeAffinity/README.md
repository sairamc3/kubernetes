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

> requiredDuringSchedulingIgnoredDuringExecution
> preferredDuringSchedulingIgnoredDuringExecution

### Planned

> requiredDuringSchedulingRequiredDuringExecution

**DuringScheduling** is a phase where the pod does not exists and it is created for the first time. 
**DuringExecution** is the phase where the pod exists in the node and the node configuration is changed, like removing the labels.

||DuringScheduling|DuringExecution|
|-|---------------|---------------|
|Type-1| Required | Ignored|
|Type-2| Preferred | Ignored|
|Type-3| Required | Required|
