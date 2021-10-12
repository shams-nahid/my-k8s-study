### PVC

---

When we crete a `Deployment` object with `Postgres` image, we create a `Pod` inside the virtual machine. The `Postgres Container` inside the `Pod` has a storage system, which is only accessible by the container itself. While the application running, `Postgres` used to store data in its container file system. Anytime the `Pod` crashed, the container along with its file system will be lost. So, after crashing a `Pod` with `Postgres`, if another `Pod` with `Container` get created, the new container can not access or represent the old crashed container data/files.

`PVC` stands for `persistent Volume Claims`. This `PVC` thing is very much similar to the docker worlds `volumes`. In docker, we used to persist volume by `Data Volume` and `Bind Mounting` to make sure using/mapping the host machine storage from the container.

We can come up architecture, where the `Postgres` will use the file system of the host machine. So in case the `Pod` crashed, and new `Pod` came up, the data in the host machine will not be lost. Also the new `Pods Container` can access these data.

1. **Volume In K8s**: In docker world, we consider `Volume` as a file system inside the host machine, used/mapped by the container. But in `k8s` world, the `Volume` is an object, used to store data in the `Pod` level. In `k8s` when we create a `Volume`, we are creating a file system inside the `Pod`. The storage can be expressed as `Belong To` or `Associated To` the `Pods`. This `Volume` can be accessed by any container inside the `Pod`. Since `Volume` is tied with the `Pods`, if `Pod` dies, terminated or recreated, for any reason, the `Volume` itself will be gone as well. So, in `k8s`, the `Volume` is not appropriate for storing database.

2. **Persistent Volume**: `Persistent Volume` is a long term durable storage, not tied to any `Pod` or `Container`. So, even a `Pod` or `Container` crashed, the data in the `Persisted Volume` survived.

3. **Persistent Volume Claim**: `Persistent Volume Claim` is an advertizement of k8s for `Statically provisioned persistent volume` and `Dynamically provisioned persistent volume`. `Statically provisioned persistent volume` are created by k8s ahead of time but `Dynamically provisioned persistent volume` are created on demand on the fly. From `Pods config file` we can ask any of these and k8s will ensure these volumes on behalf of us.

> A `Volume` is tied to Pod and can not survived if the pod delete/recreated/crashed. On the other hand, the `Persistent Volume` survived even though the `Pod` can not survive. The `Persistent Volume` only lost, if the administrator removed it.

#### Diagram Of Volume: single volume inside pod with multiple container pointing it

#### Diagram of Persistent Volume: a volume outside of the Pod, pointing multiple container
