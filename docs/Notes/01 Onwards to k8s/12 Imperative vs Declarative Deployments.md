### Imperative vs Declarative Deployments

---

Whenever we need to update/interact with a `k8s` cluster, we update/change the desired state in a config file and pass it to master. We should never directly go to the nodes directly and and update the nodes/pods/container.

The `master` is constantly working to meet the desired state.

With `Imperative Deployments`, we have to provide specific instructions step by step to reach a objective.

But when we go through the `Declarative Deployments`, we only define the final objective to the master and master will make it happen.

Whenever it comes to update status it's always good practice to update the config file and feed it to the master and follow `Declarative Deployments`.
