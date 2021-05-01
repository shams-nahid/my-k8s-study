### Local Environment for k8s

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
