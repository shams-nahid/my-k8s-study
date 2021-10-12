### Multi Docker Access

---

We have two docker installed

1. Inside local machine
2. Inside the virtual machine, created by minikube

By default our `docker-client` access the local `docker-server`. If we want to access the `docker-server` of the `virtual machine`, created by `minikube`, we can run,

```bash
eval $(minikube docker-env)
```

From this terminal, we can access the `docker-server` of the virtual machine. This is not a permanent re-configuration.

> This `eval $(minikube docker-env)` is a temporary connection of the virtual machine docker from the current terminal window. As soon as we go back to old terminal or open a new terminal, these terminal will make interaction of the local machine docker server.

Whenever we run the `eval $(minikube docker-env)`, it actually exports couple of environment variable. We can see these environment variables by,

```bash
minikube docker-env
```

This should export the variables, that helps the docker client to determine which server it is going to connect.

This command output also includes,

```
# To point your shell to minikube's docker-daemon, run:
# eval $(minikube -p minikube docker-env)
```

The second comment can guide us to configure the terminal to connect docker client with the virtual machine docker server.
