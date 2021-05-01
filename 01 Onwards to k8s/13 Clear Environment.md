### Clear Environment

---

To remove an object, we can take two approaches,

1. Delete by object name, `kubectl delete object_type object_name`
2. Delete by object file, `kubectl delete -f config_file_name`

If we want to delete by file name, we have to pass the right file name and path in as `config_file_name`.

Some examples of clearing the `k8s` objects are,

<u>**Removing Pods Object**</u> (Delete by file name)

We can remove an object from the `k8s` cluster by `kubectl delete -f config_file_name`. So the `Pod`, that is created from the config file `client-pod.yml` can be deleted by,

```bash
kubectl delete -f client-pod.yml
```

We can verify `Pods` being deleted by get the list of the Pods,

```bash
kubectl get pods
```

This is also an example of `Imperative Deployment`.

<u>**Removing Deployments Object**</u> (Delete by object name)

Get the list of deployments,

```bash
kubectl get deployments
```

This should give us all the running `Deployment` object name.

We can delete the `Deployment` object by the name,

```bash
kubectl delete deployment deployment_object_name
```

As output, we should see `deployment.apps "deployment_object_name" deleted`.

<u>**Removing Services Object**</u> (Delete by object name)

Get the list of services,

```bash
kubectl get services
```

This should give us all the running `Service` object name.

We can delete the `Service` object by the name,

```bash
kubectl delete service service_object_name
```

As output, we should see `service.apps "service_object_name" deleted`.

> In the service list, there should be a service object named `kubernetes` and it is internally used by `kubernetes` itself. We should not delete or mess with this service.

To remove the whole cluster,

```bash
minikube delete
```
