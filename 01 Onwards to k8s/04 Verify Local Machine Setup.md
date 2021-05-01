### Verify Local Machine Setup

---

We will run an docker image in the `k8s` cluster.

Check the `minikube` status,

```bash
minikube status
```

We should see the status as running and configured.

To test the cluster,

```bash
kubectl cluster-info
```

We should see the cluster is running.

For any error/warning please check the installation section.
