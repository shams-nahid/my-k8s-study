### Run Containers Inside k8s

---

Now we will run a `nginx` server in the `k8s cluster`. A single container of `nginx` can not be an objective for `k8s cluster`, but for simplicity and see how thinks get work, we are doing the demo.

Load the `Pod` config file,

```bash
kubectl apply -f client-pod.yml
```

We should see the output `pod/proxy-pod created`.

Load the `Service` config file,

```bash
kubectl apply -f client-node-port.yml
```

We should see the output `service/proxy-node-port created`.

We can get list of running pods,

```bash
kubectl get pods
```

We should see `proxy-pod` in running status.

We can get the list of services,

```bash
kubectl get services
```

We should see `proxy-node-port` in the service list.

All these services and ports are running not in the `localhost` instead, inside an IP provided by the `minikube`. We can see the provided IP by,

```bash
minikube ip
```

This should give us an IP of the `k8s cluster`.

Now, we can browse, `http://ip:31516` and see the `nginx` server is up and running.
