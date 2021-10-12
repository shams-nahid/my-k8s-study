### Secret Object Types

---

Since We should not write the secret in file/configuration, When we pass secret to the k8s it will be a imperative deployment.

`Secret` itself is a object type. Instead of write a config file, we will use `kubectl` to create this secret object.

**3 Types Of Secret**

1. **generic**:
2. **docker-registry**:
3. **tls**:

When we pass `--from-literal`, it means we are passing the secret key and value in inline format. Another option can be load the secrets from a file, which is not ideal to load secrets.

When we are using this `Imperative Deployment` to create secret, we have to execute the command for both in local environment and production cloud environment.

To create a `Secret` object, we use,

```bash
kubectl create secret <secret_type> <secret_name> --from-literal <key=value>
```

This should give us output, `secret/<secret_name> created`.

We can get the list of secrets,

```bash
kubectl get secrets
```
