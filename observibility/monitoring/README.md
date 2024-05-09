# Monitoring

There are advance tools for monitioring applications like Prometheus,Elastic stack, datadog, dynatrace etc..,

But for the certification, only metric server is required

Metric server enables you to monitor the metrics of both the `nodes` and `pods`

Every node in kuberentes will have kubelet and kubelet has a component called `cAdvisor`.
It is resposible for retrieving the perfomance metrix from the pods and transfering the data to the metric server.
The metric server stores the data in-memory and not in any external storage space. 
So there would not be any historical information there. So if you need to have such an information, then use any of the advanced tools listed in the first line.

You can install metric server in the node using 

**Minikube**
```bash
minkube addons enable metric-server
```

**others**
```bash
git clone https://github.com/kuberentes-incubator/metric-server.git
kubectl create -f deploy/1.8+/
```
All the components required will be created. 

Once installation is succesful.. give some time for the metric server to collect the information. 

You can run the command 

```bash
kubectl top node
kubectl top pod
```


