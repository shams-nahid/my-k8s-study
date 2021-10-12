### Declarative Update

---

Here we take an existing config file, leave its `metadata->name` and `kind` same, but update the image from `stephengrider/multi-client` to `stephengrider/multi-worker`.

It's not our objective to run an entire application, we will just make sure, we can change the image of the pods.

Updated the `client-pod.yaml` we deployed earlier with new image `stephengrider/multi-worker`,

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
      image: stephengrider/multi-worker
      ports:
        - containerPort: 3000
```

Now feed the new `Pod` config file to master using `kubectl` by

```bash
kubectl apply -f client-pod.yaml
```

In output, we should see `pod/client-pod configured`.

We can see all the pods list,

```bash
kubectl get pods
```

We should see the `client-pod` in the list.

We can get details of the object info by, `kubectl describe <object type> <object name>`. In our case, we can look into `client-pod` details by,

```bash
kubectl describe Pod client-pod
```

In the details output, under `Containers` we should see `Image: stephengrider/multi-worker`.

This is the exact image we used in the update config file. Also this can be considered as a declarative deployment.
