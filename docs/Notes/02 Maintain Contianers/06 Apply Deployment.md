### Apply Deployment

---

Before we deploy the `client-deployment.yaml`, make sure existing `Pods` are being deleted. We can verify no Pods is being running by `kubectl get pods`.

Now we can deploy and create a `Deployment Object` by,

```bash
kubectl apply -f client-deployment.yaml
```

We should see an output `eployment.apps/client-deployment created`.

If we get the list of running pods,

```bash
kubectl get deployments
```

We should see a `Deployment` object named `client-deployment`.
