### Deployment Object Types

---

Since, we have some limitations of using `Pod` for `Declarative Deployment`, we can make use of another types of object `Deployment`. `Deployment` object is meant to maintain a set of identical Pods. It keep the states of the `Pod` updated and ensure number of `Pods`.

**Pod Vs Deployment**

Both, `Pod` and `Deployment` are object types of `k8s`.

<u>Pods</u>

1. Run single set of tightly coupled containers
2. Used in dev/local environment

<u>Deployment</u>

1. Runs set of identical `Pods`
2. Monitor and keep the updated state of the `Pods`
3. Good for dev and production environment

When we create a `Deployment` object, we have to attach a `Pod Template`. A `Pod Template` is a block of `Pod` configuration. So essentially, every `Pod` we run with the `Deployment` object will follow the provided template configuration.

Any time we change the config of the `Pods Template`, the `Deployment` first try to update the `Pod` configuration. If it fails to update `Pod` according to the updated `Pods Template` in this case, it will kill the existing `Pod` and create brand new `Pod` with updated template config.
