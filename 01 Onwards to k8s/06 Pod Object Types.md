### Pod Object Types

---

Whenever we start the `minikube` by `minikube start`, we started a virtual machine in the local machine. We call this virtual machine `Node` in terms of `k8s`.

`Pod` is one of the most basic objects. `Pod` essentially is a grouping of containers of very similar purpose. Containers in a single `Pod` must be very tightly coupled and must be executed with each other. For example, if we have an application with `API` container and `frontend` container, we might not want to put them in a single `Pod`. But if there is a container for `DB` and another for `Logging the DB`, in this case both container should go inside a same `Pod`.

In `k8s` we can not run a bare bone container without any overhead associate. A `Pod` is the minimum overhead to deploy a container in the `k8s` cluster. Inside `Pod` we have to run at least one or more container in it.

When we push the `Pod` config file using `kubectl` to the `k8s` engine, it eventually create a `Pod` inside the `Node` aka `Virtual Machine`.

We define which container will run inside the `Pod`, in the config file under `spec -> containers`.
