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


## User Accounts

Kuberentes does not manage user accounts natively. It relies on the external source like file with user details or certificates or 3rd party indentity services like LDAP to manage these users. So you cannot create users in the kuberenetes cluster. 

However, in case of service accounts kubernetes can manage them.

The `kube api server` authenticates the request before processing it. 
This can be done in multiple ways. 

**Static Password File:** Store user name and password in the file in csv format. 
password,username,userid,group

**Static Token file:** Store tokens and username in the file in cs format. 
token,username,userid,group

The path of this file should be passed to api server during startup in the `cmd` field
```bash
--basic-auth-file=<path of the file>
```
