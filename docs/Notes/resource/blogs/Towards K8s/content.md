Before we go further, we will try to find out the answers two most important question,

- What is Kubernetes?
- Why we use Kubernetes?

> In short form, we can refer `Kubernetes` as `k8s`. We can spell `k-eight-s`. Eight is here to refer eight letter between first and last letter of `kubernetes`.

Let's assume we a running multiple containers in the `Elastic Beanstalk`. If we started to get a lot of users to our application, we might need to scale the application. When we do the scaling, it is important to keep in mind, we only scale the container who took the loads of the traffic, not all the containers. But with `Elastic Beanstalk` this type of scaling will be real challenging.

Let's find out how k8s can solve this entire scaling issue.

In k8s, there is `k8s cluster` that is made up with `master + nodes`.

Here, **nodes** can be virtual machine or physical computer contains some number of different containers. And, **master** controls each of the nodes.

So with a `k8s cluster`, we can have a `node` that will contain some containers these do not have to scale. And a bunch of other `nodes` contains the worker process container, need to scale with traffics, they will be scaled according to the traffic.

The `master` is running couple of programs to determine how the nodes will run all the containers on our behalf. As developer, we interact the `k8s cluster` only by the `master`. We send the instructions to the `master` and the `master` will pass all these instructions to the respective `nodes`.

So the bottom line is, we can summarize the answer

- What is k8s?

> A container orchestrator make many servers act like one. Kubernetes is one of the most popular container orchestrator.

- Why we use k8s?

> Deploy the containerized apps.

### k8s Tools

---

**kubernetes**: `K8s` is the short form of `kubernetes`. It's the whole orchestration system.

**kubectl**: `kubectl` is an abbreviation of `kube control`. It's the `CLI` for k8s and used to configure and manage the apps.

**Nodes**: Single server/virtual machine in the k8s cluster.

**kubelet**: An small agent, run in the `Node` to communicate between `Node` and the `master`.

**Control Plane**: Also known as `master`. `Control Panel` or `master` is in the charge of whole cluster. It make sure, the current state of the cluster is matched with the desired state.

### Local Environment for k8s (Ubuntu)

---

To run k8s in the local environment, we will need two programs installed in the machine,

- Minikube
- kubectl

**Install Minikube**

Download the program,

```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
```

To install,

```bash
sudo install minikube-linux-amd64 /usr/local/bin/minikube
```

This is optional. If the user is not added to the `docker group`,

```bash
sudo usermod -aG docker $USER && newgrp docker
```

Start Minikube by

```bash
minikube start
```

To check if minikube is running, check the status,

```bash
minikube status
```

**Install kubectl**

Download the program,

```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
```

To install

```bash
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
```

To test if the `kubectl` is installed, check the version,

```bash
kubectl version
```

### Verify Local Machine Setup

---

We will run an docker image in the `k8s` cluster.

Check the `minikube` status,

```bash
minikube status
```

We should see the status as running and configured.

To test the cluster,

```bash
kubectl cluster-info
```

We should see the cluster is running.

For any error/warning please check the installation section.

### k8s Development Flow

---

In the `k8s` world, we pass the config files like `Pod` or `Service` to the `master` using the `kubectl`. The `master` has a responsibility to determine how many container is being running inside the pods in various nodes.

We can define which nodes will run which container and how many. The master monitor all these containers inside the `Pod` of the `Node`. If some nodes fails to run or crash on runtime, the master will run another to keep the expectation.

### Imperative vs Declarative Deployments

---

Whenever we need to update/interact with a `k8s` cluster, we update/change the desired state in a config file and pass it to master. We should never directly go to the nodes directly and and update the nodes/pods/container.

The `master` is constantly working to meet the desired state.

With `Imperative Deployments`, we have to provide specific instructions step by step to reach a objective.

But when we go through the `Declarative Deployments`, we only define the final objective to the master and master will make it happen.

Whenever it comes to update status it's always good practice to update the config file and feed it to the master and follow `Declarative Deployments`.

### k8s in Production

---

When we choose to use k8s as orchestrator, then we need to choose the vendors for it.

We can go either `Cloud Managed Solution` like `GCP`, `AWS`, `Azure`, `Digital Ocean` etc. We can also choose self managed distribution like, `Docker Enterprise`, `Rancher`, `OpenShift`, `Canonical`, `VMWARE PKS` etc.
