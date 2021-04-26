### Update Existing Objects

---

When we need to update existing cluster state, we can either go imperative or declarative deployment.

If we are running a cluster with a `Pod` object and `Service` object. In declarative deployment, when we need to update the cluster, we have to make sure the `name` and `kind` of the object should be same. We will write the updated state in the config file. With `kubectl` we will feed the `master` updated config file, and the `master` will do the rest.

To do a hands on, first make sure we have a `Pod` and `Service`. The `Pod` config file `client-pod.yaml` is,

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: client-pod
  labels:
    component: web
spec:
  containers:
    - name: client
      image: stephengrider/multi-client
      ports:
        - containerPort: 3000
```

Feed this `Pod` object to the `master` by,

```bash
kubectl apply -f client-pod.yaml
```

Amd the `Service` config file `client-node-port.yaml` is,

```yaml
apiVersion: v1
kind: Service
metadata:
  name: client-node-port
spec:
  type: NodePort
  ports:
    - port: 3050
      targetPort: 3000
      nodePort: 31515
  selector:
    component: web
```

Feed this `Service` object to `master` by,

```yaml
kubectl apply -f client-node-port.yaml
```
