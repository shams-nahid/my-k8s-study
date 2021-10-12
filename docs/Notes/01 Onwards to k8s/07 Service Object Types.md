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
