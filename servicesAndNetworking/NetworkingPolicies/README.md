# Network policies

Suppose you have a simple architecture. 

Traffic -> Web Server -> App Server -> DB

All the 3 services are deployed in the same cluster. The servies would communicate with on another.

- Web Server -> port 80
- App Server -> port 8080
- Db -> port 3060

By default in a kuberents cluster all the pods are enabled to communicate with each other. In which case, Web server also could communicate with the DB. You don't want that. You just want the App Server to communicate with the Db Server.

This is where you introduce network policies to restrict the communication. 

There are two types of traffic
- Ingress (Traffic Towards the server)
- Egress (Traffic From the server)

In the above specified configuration, Ingress and Egress would be something like this

An example of the NetworkPolicy is provided in the [Network Policy](network-policy.yaml)

### namespaceSelector

Here without the namespace selector any pod from any namespace with matching label can communicate. If we need to restrict only the prod namespace pods only should be able to communicate with the prod db, then we need to use the `namespaceSlector`. Since we are using the `matchLabels`, we should label the namespace as well. 

### ipBlock

Suppose you connect your db to external backup server which is not part of the cluster, this is how will enforce the policy to allow the connection to the backup server. 

Solutions that support network policies are:
- kube-router
- calcio
- romana
- weave-net

Solutions that does not support
- Flannel



