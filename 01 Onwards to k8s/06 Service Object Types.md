### Service Object Types

---

`Service` is all about setting up networking in the `k8s cluster`. `Service` types have 4 sub-types,

- ClusterIP
- NodePort
- LoadBalance
- Ingress

We define the sub-types under the `spec` property.

**NodePort**

`NodePort` is supposed to expose the container to the outside world. In most cases, we use `NodePort` inside the dev stage.

There's a `selector` under the `spec` also. In the `Service` config file there is no reference of the `Pod` config file. In the `Pod` config file, we have a `name` and `labels` under the `metadata`. To send traffic to the `Pod` from the `Service`, we do not see any reference of the `Pod`. Instead in `k8s`, we use the `label selector` mechanism. In the `Service` file we have a selector under `component: web`, which is same as the `Pod` file labels, `component: web`. This is how the `Service` knows the `Pod` to send traffic.

After selecting the `Pod` from the `selector`, comes the ports, under the `spec -> NodePort -> ports` we got 3 types of ports,

1. **ports**: Internally other `Pod` connect through this port.
2. **targetPort**: This should be same as the `containerPort` in the `Pod` config file. This is the open port of the container inside the `Pod`.
3. **nodePort**: This is the port our application is exposed to the outside world. This port ranges 30000 to 32767. If we do not specify, it will generate a random port within this range.
