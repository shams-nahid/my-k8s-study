### Combining Config Files

---

There's two ways we can organize the k8s config files in a directory.

1. **Put multiple config files in a single file**: If we put multiple config files, we have to put a `---` between two config files. With this architecture, we can put close objects configuration together. But the downside is, an engineer have to actively understand the tightly coupled services for any changes.
2. **Put each config file in individual file**: In this case, each object config will have an individual file with meaningful name. So any time, if there is a requirement to change/update the config it is easy to find. The downside is, there could be a lot of files of configuration.
