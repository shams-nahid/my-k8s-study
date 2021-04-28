### Updating Deployments

---

Here we update the deployments config file and verify the associated Pods are updating accordingly.

To do so, lets update the configuration file of the `client-deployment.yaml` with `containerPort: 9999`. The updated `client-deployment.yaml` should be,

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: client-deployment
spec:
  replicas: 1
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

Now, we feed the updated config file to the `master` by the `kubectl`,

```bash
kubectl apply -f client-deployment.yaml
```

We should see output `deployment.apps/client-deployment configured`. This updated state essentially kill the existing Pod and create a new Pod with updated configuration.

We can see the list of deployments object,

```bash
kubectl get deployments
```

We can see the updated/new Pod object by,

```bash
kubectl get pods
```

Since, with new configuration, we have to kill the exiting Pod and generate a new one, the pods age should be relatively very small.

Now it is important to verify, the new Pod has the container port of `9999`. We can get pods details,

```bash
kubectl describe pods object_name_created_by_client_deployment_config_file
```

Under `containers -> Client`, we should see `Port: 9999/TCP`. So without doubt, the Pod is running with up to date configuration.
