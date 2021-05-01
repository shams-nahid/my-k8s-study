Kubernetes is out there to orchestrate our containers to a desired state. We notify the kubernetes engine our desired application state with a configuration files. We feed these configuration files to `kubectl` and `kubectl` pass them to the `master` or the `control plane`. Here theses config files becomes object/controller and helps the master to keep the application always in the desired state.

According to the `Kubernetes Doc`,

> `Kubernetes` objects are persistent entities in the `Kubernetes` system. `Kubernetes` uses these entities to represent the state of your cluster.

In `k8s` there are couple types of objects, for example,

- StatefulSet
- ReplicaController
- Pod
- Service etc.

These objects have different purposes, like,

- Running a container
- Monitoring
- Setting up networking etc.

In this article we will discuss very commonly used object types, `Pod`, `Service` and `Secret`.

### Pod Object Types

---

Whenever we start the `minikube` by `minikube start`, we started a virtual machine in the local machine. We call this virtual machine `Node` in terms of `k8s`.

`Pod` is one of the most basic objects. `Pod` essentially is a grouping of containers of very similar purpose. Containers in a single `Pod` must be very tightly coupled and must be executed with each other. For example, if we have an application with `API` container and `frontend` container, we might not want to put them in a single `Pod`. But if there is a container for `DB` and another for `Logging the DB`, in this case both container should go inside a same `Pod`.

In `k8s` we can not run a bare bone container without any overhead associate. A `Pod` is the minimum overhead to deploy a container in the `k8s` cluster. Inside `Pod` we have to run at least one or more container in it.

When we push the `Pod` config file using `kubectl` to the `k8s` engine, it eventually create a `Pod` inside the `Node` aka `Virtual Machine`.

We define which container will run inside the `Pod`, in the config file under `spec -> containers`.

### Service Object Types

---

`Service` is all about setting up networking in the `k8s cluster`. `Service` types have 4 sub-types,

- NodePort
- ClusterIP
- LoadBalancer
- Ingress

We define the sub-types under the `spec` property.

<u>**NodePort**</u>

`NodePort` is supposed to expose the container to the outside world. In most cases, we use `NodePort` inside the dev stage.

There's a `selector` under the `spec` also. In the `Service` config file there is no reference of the `Pod` config file. In the `Pod` config file, we have a `name` and `labels` under the `metadata`. To send traffic to the `Pod` from the `Service`, we do not see any reference of the `Pod`. Instead in `k8s`, we use the `label selector` mechanism. In the `Service` file we have a selector under `component: web`, which is same as the `Pod` file labels, `component: web`. This is how the `Service` knows the `Pod` to send traffic.

After selecting the `Pod` from the `selector`, comes the ports, under the `spec -> NodePort -> ports` we got 3 types of ports,

1. **ports**: Internally other `Pod`/`object` connect through this port.
2. **targetPort**: This should be same as the `containerPort` in the `Pod` config file. This is the open port of the container inside the `Pod`.
3. **nodePort**: This is the port our application is exposed to the outside world. This port ranges 30000 to 32767. If we do not specify, it will generate a random port within this range.

**\*\*** Diagram of port mapping of theses 3 ports by local machine, cluster, container, other objects

<u>**ClusterIP**</u>

It is a restrictive form of networking. It allows any objects inside the cluster to access the object it is pointed to. But outside ot the cluster, like from browser, it does not allow to access that object.

Practically, `ClusterIP` allows anyone to access the object. Without this service, the object can not be accessed and inf we use `NodePort` service instead, it will expose the object to the outside world of the k8s cluster.

It has 2 types of port,

1. **Port**: In the `k8s cluster`, other objects can connect to the clusterIp service container using this port.
2. **targetPort**: The `ClientIP Service` is pointing to the container with this `targetPort`.

**\*\*** Diagram of port mapping of theses 3 ports by local machine, cluster, container, other objects

<u>**Load Balancer**</u>

This is an older way to handle traffic from outside to k8s cluster. With `Load Balance Service`, we replace the `ClusterIp Service` with it. And the k8s reach to the cloud provider to get there built in load balancer (For example, in AWS, it could be Classic Load Balancer or Application Load Balancer). When we use `Load Balancer Service`, it only allows the k8s cluster to expose only a set of specific Pods. The limitations are, it can not expose multiple set of pods.

<u>**Ingress Service**</u>

This Ingress service allows to come traffic inside the `k8s cluster`. When the traffic is inside the `k8s cluster`, effectively these traffic can also access the objects through ClusterIP` service.

### Ingress Service

---

When it comes to ingress service in k8s, there are two options we have to consider,

1. [kubernetes-ingress](https://github.com/nginxinc/kubernetes-ingress): Maintained by the `Nginx` company itself.
2. [ingress-nginx](https://github.com/kubernetes/ingress-nginx): A community driven project.

Comparatively, ingress nginx can be a [better choice](https://www.joyfulbikeshedding.com/blog/2018-03-26-studying-the-kubernetes-ingress-system.html), we will use and discuss about the `ingress-nginx` here.

Difference between them are discussed [here](https://github.com/nginxinc/kubernetes-ingress/blob/master/docs/nginx-ingress-controllers.md).

When it comes to set up `ingress-nginx`, it varies for different cloud provider.

**Controller**: In k8s a controller is a kind of object, that constantly works to make the current state to the desired state. For example, when we create a `Deployment Object` using the config file, the `Deployment Object` observe the k8s cluster current state and ensure it to match the desired state.

In a `ingress service`, we create a config file with some routing rules for the incoming traffic and send it off to the appropriate service inside the cluster. Now the `kubectl` create the `ingress controller`. Internally this `ingress service` is using `Nginx`. So this `ingress controller` will create a `Pod` with `Nginx Controller` in it. With this `Nginx` container the routes being handled and passed to different set of `Pods`.

**Ingress Nginx In Google Cloud**

As always, we have to create the config file of the `ingress service`. This config will be passed to a `Deployment` where the `ingress controller` and the `Nginx Pod` both will run. This `Nginx Pod` will take the incoming traffic and pass them to the appropriate set of `Pods`. In `Google Cloud`, when we create a `ingress service`, a load balancer `Google Cloud Load Balancer` will also be created for us. This `Google Cloud Load Balancer` is a cloud native service. When the `Google Cloud Load Balancer` is created outside of the cluster, a `Load Balancer Service` will also be created inside the `k8s cluster`.

We know the `Load Balancer Service` is only capable of distribute traffic only a set of `Pods`. So here the `Load Balancer Service` will pass all these traffic to `Nginx Pod` and `Nginx Pod` will take care of the rest of traffic distribution between `Pods`.

In dev, there is an additional deployment object, `default-backend-pod`, to ensure the health check of the other `Pods`. In production this will be replaced by the original application.

**Why ingress-nginx?**

We can simply make use of a `Google Cloud Load Balancer`, `Load Balancer Service` and `Plain Nginx Pod`. But we make use of the `ingress-nginx` because it is optimized and built for run in such `k8s cluster` environment. For example, when we are using `Nginx Pod`, it can bypass the `Cluster Ip Service` and pass traffic directly to the `Pod`. This feature is required for `sticky session`.

> `Sticky Session` enables the user to a client always communicate with the same server.

**[Enabling Ingress](https://kubernetes.github.io/ingress-nginx/deploy/#verify-installation)**

To enable the `ingress` service in `minikube`,

```bash
minikube addons enable ingress
```

In output, we should see `The 'ingress' addon is enabled`.

To verify, if the `ingress` service is enabled or not,

```bash
kubectl get pods -n ingress-nginx -l app.kubernetes.io/name=ingress-nginx --watch
```

To detect, which version is being installed,

```bash
nginx-ingress-controller --version
```

### Secret Object Types

---

Since We should not write the secret in file/configuration, When we pass secret to the k8s it will be a imperative deployment.

`Secret` itself is a object type. Instead of write a config file, we will use `kubectl` to create this secret object.

**3 Types Of Secret**

1. **generic**: Regular application secrets, like DB_PASSWORD, JWT_SECRET, API_SECRET etc.
2. **docker-registry**: Create secrets to use for the `Docker Registry`
3. **tls**: Used to store certificates and associated keys

When we pass `--from-literal`, it means we are passing the secret key and value in inline format. Another option can be load the secrets from a file, which is not ideal to load secrets.

When we are using this `Imperative Deployment` to create secret, we have to execute the command for both in local environment and production cloud environment.

To create a `Secret` object, we use,

```bash
kubectl create secret <secret_type> <secret_name> --from-literal <key=value>
```

This should give us output, `secret/<secret_name> created`.

We can get the list of secrets,

```bash
kubectl get secrets
```
