### Creating Configurations

---

To create a secret for the postgres password,

```bash
kubectl create secret generic pgpassword --from-literal PGPASSWORD=1234asdf
```
