# Helm

Helm works as a package manager for kubernetes. We have a single application and in order to make it work we need to have multiple resources working together in kuberentes. Now each resource has a specific yaml file. which makes a very large number of yaml files for a single application. 

It's hard to manage those files. That's where helm comes in.

You can use helm to install multiple components at once, upgrade, rollback, uninstall all the components considering al l the components of tha application as one. 

```bash
helm install wordpress ...
helm upgrade wordpress ...
helm rollback wordpress ...
helm uninstall wordpress ...
helm repo add <repo-url>
helm search hub wordpress
helm search repo wordpress
helm pull wordpress
```


