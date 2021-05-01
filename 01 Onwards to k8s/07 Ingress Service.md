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
