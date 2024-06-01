# Security

Let us begin with host that forms the cluster itself. Ofcourse, all access to these hosts must be secured, root access disabled, password-based authentication disabled and only SSH key based authentication to be made available. Along with any other measure that you take to protect you physical or virtual infrastructure that hosts kubernetes. 

> If it is compromised, everthing is compramised. 

The focus here is mostly kubernetes related security. 

The `kube api server` is center of all operations with in the kuberenetes. We interact with it using `kubectl` or by accessing the api directly. And through that, you can perform almost any activity in the cluster. So it is our first line of difence. 

## Authentication

Who can access the api server is defined by authentication.
- username and password stored in static files
- tokens
- certificates
- external authentication providers like LDAP

## Authorization

Who can access what?

Authoization is implemented is based `role based access control`, where users are associated to groups with specific permissions. In addition to that there are other modules like
- Attribute based access control
- node authorizors
- web pods

All the communication between various components such as etcd cluster, the kube controller, manager scheduler, api server, as well as those running in the worker nodes such as kubelet and kube-proxy is secured using TLS Encryption. 

