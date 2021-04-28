### Updating Deployment Image

---

In the world of `k8s` recreate the Pods for the updated version of image is quite complex. There's a popular [issue](https://github.com/kubernetes/kubernetes/issues/33664) in github on this topic. Traditionally we update/make change of the deployment file, we used to send the updated deployment file to the `master` using the `kubectl` command.

In the deployment config file, there is no particular version of the image. So In case the image is updated in the docker hub, but the config file is not changed. `kubectl` does not change any changes in config and simply reject the file.

`k8s`, on our behalf does not go and check if a new version of the image is available or not.

This is why this is such a big issue.

With this in our mind, we can consider 3 possible issues,

**Deleting Pod :** We can simply delete the Pods. In this case, for the missing Pod, the master will create the Pods to match the desired state. So this time hopefully the latest image will be pulled in. We should get Pods with latest version of images.

**Image Tagging :** We can tag the images while building and also specify the version in the deployment config file.

**Using Imperative Command :** In this case, we will put image tag every time we build a new image. But instead of putting the image tag in the deployment config file, we will use `imperative command` to update the image version.

None of these solutions are ideal. If we compare all these 3 options, we can definitely see the first one is very bad practice. Second one of `Image Tagging` is a pain for updating two config files every time. Among them `using imperative command` is comparatively much more usable.

Here we will look into how can we update the deployment when a new version becomes available. We deploy the `deployment` with `stephengrider/multi-client` image, 3000 port and 1 replicas. Then we update the `multi-client` image and push the updated image to docker hub. Now ask the `Deployment Object` to recreate the Pods with the updated states.

Our initial deployment config file `client-deployment.yaml` be like,

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
            - containerPort: 3000
```

Feed this initial state to the `k8s cluster` by,

```bash
kubectl apply -f client-deployment.yaml
```

We should see output `deployment.apps/client-deployment configured`.

To access the site, we first get the ip of the virtual machine,

```bash
minikube ip
```

This should output the IP of the virtual machine.

Now browse `http://virtual_machine_ip:31515/` we should see the react app is running.

Now update the image. Let's say we are building new image with tag `v5`.

We can use `imperative command` to update the image inside the cluster using the format,

`kubectl set image <object_type> / <object_name> <container_name> = <new_image>`

In our case, we can interpret as,

```bash
kubectl set image deployment/client-deployment client=stephengrider/multi-client:v5
```

Now in output, we should see `deployment.apps/client-deployment image updated`.

We can see the pods,

```bash
kubectl get pods
```

We should notice the `AGE` of the pods is relatively small. This means without any doubt, the Pod is being recreated by the deployment.

If we now browse `http://virtual_machine_ip:31515/` we should see the react app is running and the title is changed to `Fib Calculator version 2`

This entire process is a little bit of pain.

- We have to rebuild the image
- Have to put a tag to the image
- Run an `imperative` command to update the image

In production, there should be a script to handle these issues automatically.
