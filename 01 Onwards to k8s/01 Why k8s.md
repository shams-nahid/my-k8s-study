### Why k8s

---

Before we go further, we will try to find out the answers two most important question,

- What is k8s?
- Why we use k8s?

Let's assume we a running multiple containers in the `Elastic Beanstalk`. If we started to get a lot of users to our application, we might need to scale the application. When we do the scaling, it is important to keep in mind, we only scale the container of the worker process, not all the container. But with `Elastic Beanstalk` this type of scaling will be real challenging.

Let's find out how k8s can solve this entire scaling issue.

In k8s there is k8s cluster that is made up with `master + nodes`.

Here, **nodes** can be virtual machine or physical computer contains some number of different containers. And, **master** controls each of the nodes.

So with a `k8s cluster`, we can have a `node` that will contain some containers these do not have to scale. And a bunch of other `nodes` contains the worker process container, they will be scaled according to the traffic.

The `master` is running couple of programs to determine how the nodes will run all the containers on our behalf. As developer, we interact the `k8s cluster` only by the `master`. We send the instructions to the `master` and the `master` will pass all these instructions to the respective `nodes`.

There will be a `Load Balance` in front of the `k8s cluster`.

So the bottom line is, we can summarize the answer

- What is k8s?

> A container orchestrator make many servers act like one. Kubernetes is one of the most popular container orchestrator.

- Why we use k8s?

> Deploy the containerized apps.

<h3>SWARM And K8s</h3>

---

**SWARM**

- +Single vendor container platform, comes with docker
- +Easiest orchestrator
