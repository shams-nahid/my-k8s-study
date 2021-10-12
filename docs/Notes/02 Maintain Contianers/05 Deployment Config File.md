### Deployment Config File

---

Here, we create a config file for the `Deployment` object and feed the config file using `kubectl` to the `master`.

Like previous, we are going to use a `Pod Template` that is responsible to run the `multi-client` app.

Our deployment file `client-deployment.yaml` should be as follows,

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: client-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      component: web
  template:
    metadata:
      labels:
        component: web
    spec:
      containers:
        - name: client
          image: stephengrider/multi-client
          ports:
            - containerPort: 3000
```

**apiVersion:** We are using the `Deployment` object, that is defined in the `apps/v1` api

**Kind :** The object type is `Deployment`

**metadata -> name :** This will be the created deployment object name `client-deployment`

**spec -> selector :** With this selector, the `Deployment` object can handle the `Pod`. After a `Pod` is being create, the `Pod` also have a `select-by-labels` name. This should be same as the `spec -> selector` of the `Deployment` object.

**spec -> replicas :** Number of pods will be created by the `Deployment Object`

**spec -> template :** Config of the pods, will be used for every single pod we will create and maintain using the `Deployment Object`. This template is very much similar to the `Pod` config file.
