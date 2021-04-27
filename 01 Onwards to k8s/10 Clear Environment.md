### Clear Environment

---

We can remove an object from the `k8s` cluster by `kubectl delete -f config_file_name`. So the `Pod`, that is created from the config file `client-pod.yml` can be deleted by,

```bash
kubectl delete -f client-pod.yml
```

We can verify `Pods` being deleted by get the list of the Pods,

```bash
kubectl get pods
```

This is also an example of `Imperative Deployment`.
