### Applying Configurations

---

We are putting all the configurations in the `k8s` directory. With `kubectl`, instead of file, if we pass a directory as `apply` command, it will automatically look into the directory config files and apply them all on our behalf.

To apply all the config files inside `k8s`, we can run,

```bash
kubectl apply -f k8s
```

As output, we should see the list of created/configured/unchanged object.

---

We can check the deployment objects,

```bash
kubectl get deployments
```

This should give us the output of deployment objects.

---

We can check the pod objects,

```bash
kubectl get pods
```

This should give us the output of pod objects.

---

We can check the pod objects,

```bash
kubectl get pods
```

This should give us the output of pod objects.
