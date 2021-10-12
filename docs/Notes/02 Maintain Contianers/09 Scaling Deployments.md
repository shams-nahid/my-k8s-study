### Scaling Deployments

---

If we have a requirement to use `5 Pods` instead of `1 Pod`, we can update the deployment config files `replicas` to `5` by `replicas: 5`.

Our scaled deployment config file, `client-deployment.yaml` should be following,

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: client-deployment
spec:
  replicas: 5
  selector:
    matchLabels:
      component: web
  template:
    metadata:
      labels:
        component: web
    spec:
      containers:
        - name: client
          image: stephengrider/multi-client
          ports:
            - containerPort: 9999
```

Now feed the config file to `master` using `kubectl` by,

```bash
kubectl apply -f client-deployment.yaml
```

We should get output `deployment.apps/client-deployment configured`.

```bash
kubectl get deployments
```

This should listed a deployment object with `5/5`, i.e. 5 pods are desired and 5 pods are running.

Now if we look for the pods list,

```bash
kubectl get pods
```

We should see 5 Pods in running state, previously it was only 1 Pod.
