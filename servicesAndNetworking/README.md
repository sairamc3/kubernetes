# Services

Kubernetes services enable communication between various components both with in and outside of the application. 

Services enable 
* Users to connect to the pods
* Communication between the pods ( grouped together like front-end, back-end, web etc.,)
* Commnnication to external services ( like db)

## External communication

We have deployed a pod and a web application is running in it. How do we as an external user access the webpage?

Let's look at the existing setup.

* The k8 node has an ip address `192.168.1.2`
* My laptop is on the same network and it's ip address is `192.168.1.10`
* The internal pod network is in the range `10.244.0.0`
* The pod has an ip `10.244.0.2`

Clearly I cannot access the pod using it's ip address, as it's in a separate network. What are the options to see the webpage?

* We can do ssh to the kuberentes node 

```bash
ssh 192.168.1.2
```
* From the nodes we will be able to access the pods, by doing a curl

```bash
curl http://10.244.0.2/
```

I don't want to do it. I want to access the webpage from my local machine. So we need something to map the request to the node to the pod. 

Laptop --> Node --> Pod

This is where kubernetes **Service** comes into play.

* A **Service** is an object just like a replicaset, deployement etc.,
* one of it's use case is to listen to a node port and forward the request to a port in a pod.
    - this type of service is called `NodePort` Service

## Service Types

1. NodePort
2. ClusterIP
    - The service creates a virtual ip inside the cluster to enable communication between different services such as a set of front end servers to a set of back end servers. 
3. LoadBalencer
    - It provisions a load balencer for our application in supported cloud providers.
    - To distribute load accross different web servers in the front end tier

### NodePort

There are 3 ports involved. These terms are from the view point of the service. The service is infact like a virtual server inside the node. Inside the cluster it has it's own ip address. That ip address is called **ClusterIP** of the service.
1. the port on the pod, where the actual web server is running -> **targetPort**
    - This is not mandatory field.
    - if not defined, then the `targetPort` will be considered same as `port`
2. the port on the service, it is simply refered to -> **port**
    - This is the mandatory field.
3. the port on the node, which we use to access the web server externally and that is known as the -> **nodePort**
    - In the example it is set to `30008`
    - Node ports can only be in a valid range. 
    - The range by default is from
        - **30000 - 32767**
    - This is not mandatory
    - If undefined, a valid port will be assigned from the node, with in the range defined above.

[NodePort Service Example](nodePort-service.yaml)

The ports is plural in the file, it's an array. Hence, you can define multiple ports in the same service. 

## Mapping a service to a pod

* We use labels and selectors to connect a service to a pod. 

```yaml
selector:
    app: myapp
    type: frontend
```
* If there are multiple pods and the service maps to all the pods, then the load is shared between the pods. 
* The traffic is routed in random.
* When the pods are distributed accross multiple nodes, when we create a service, kubernets creates a service that spans accross those nodes and maps the target ports to the same nodeport on all the nodes in the cluster. This way you can assess your application using any node in the cluster and using the same port. 
* when pods removed or added, the service is automatically updated 

## ClusterIP

* A full stack web application can have multiple set of pods doing a part of the functionality. Like
    - front-end
    - back-end
    - redis
* A front-end server needs to communicate with back-end server 
    - and backend server needs to communicate with redis server
* What is the right way to establish connectivity between these services
* Each pods will have ip addresses assigned to them, and those pods are dynamic, they can be scaled up or down, in which case there will be new ip adresses or exiting ones will become invalid. 
* So you cannot rely on the ip addresses for communication
* A kuberenetes **Service** can group together certain pods and provide an interface for communicating with those group of pods. 
* A service created for back-end pods will help to communicate with them without knowing thier internal ip addresses.
* The requests are forwarded to one of the pods randomly
* This type of service is known as **ClusterIP**
* It is the default type if the type is undefined in the `spec`

[ClusterIP Service example](clusterIp-service.yaml)

