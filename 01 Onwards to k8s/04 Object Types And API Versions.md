### Object Types And API Versions

---

In `k8s` world, apart from `docker-compose`, we need to consider,

- `k8s` expect all the containers image already be built
- In `k8s cluster` we have to manually do the networking

Our `client-pod.yml` should be like the following,

```yml
apiVersion: v1
kind: Pod
metadata:
  name: proxy-pod
  labels:
    component: web
spec:
  containers:
    - name: nginx-proxy
      image: nginx
      ports:
        - containerPort: 80
```

And our `client-node-port.yml` should be like the following,

```yml
apiVersion: v1
kind: Service
metadata:
  name: proxy-node-port
spec:
  type: NodePort
  ports:
    - port: 3050
      targetPort: 80
      nodePort: 31516
  selector:
    component: web
```

In `k8s` world, when we are making a config file, we are not essentially making a `container`, instead we are making a object. These object/thing is being created inside the `k8s` engine to make the

**kind**

We made two configuration file, `client-pod.yml` and `client-node-port.yml`. We will feed these two configuration file to the `kubectl`. `kubectl` will take these two config file and create two objects out of them. In `k8s` there are couple types of objects, for example,

- StatefulSet
- ReplicaController
- Pod
- Service etc.

These objects have different purposes, like,

- Running a container
- Monitoring
- Setting up networking etc.

In our config file, we define the types of the object by the `kind` property. For example, in the `client-pod.yml` file, we set the object type `Pod` in the `yml` file by `kind: Pod`. similarly, in the `client-node-port.yml` file, we define the object type to `Service` by `kind: Service`.

**apiVersion**

Each `apiVersion` has a set of object types. For example, when the `apiVersion` is `v1`, we can specify the `objectTypes` or `kind` as,

- componentStatus
- configMap
- Endpoints
- Event
- Namespace
- Pod

Instead, if the `apiVersion` is `apps/v1`, we can use `objectTypes` or `kind` as,

- ControllerRevision
- StatefulSet

> Each API version has different set of objects type. So before we using any object types, we have to check the api Version and specify on the top of the `yml` file.
