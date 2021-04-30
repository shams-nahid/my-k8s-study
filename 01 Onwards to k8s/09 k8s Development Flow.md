## k8s Development Flow

---

In the `k8s` world, we pass the config files like `Pod` or `Service` to the `master` using the `kubectl`. The `master` has a responsibility to determine how many container is being running inside the pods in various nodes.

We can define which nodes will run which container and how many. The master monitor all these containers inside the `Pod` of the `Node`. If some nodes fails to run or crash on runtime, the master will run another to keep the expectation.
