# ENV VARIABLES IN KUBERNETES

To set an environment property we use the **env** property within _containers_ withing _spec_. This proeprty will be an array which will have a _name_ and a _value_ properties. (see file _pod-definition.yaml_)

However, there are other ways of setting the environment variables, such as using _ConfigMap_ and _Secrets_. The difference is that instead of specifying _value_ we specify _valueFrom_, and then the specification of _configMap_ or _secret_.

### ConfigMap

When you have a lot of Pod definition files it will become difficult to manage the environment data. For that, we can take this information out of the definition file and manage them centraly using configuraiton maps.

Config maps are used to passed configuration data in the form of key value pairs in kubernetes: When a Pod is created inject the config map into the Pod so the key value pairs are availabe as environment variables for the application hosted inside the container in the Pod.
There are two faces involved in configuring Config Maps:

1. Create the config map. There are two ways of creating a config map:

- Imperative way. With the command `kubectl create configmap` and passing to it the necessary arguments:
  - _config-name_
  - _--from-literal=key=value_. Example `kubectl create configmap app-config --from-literal=APP_COLOR=blue`. If we want to specify more, we need to add the _from-literal_ argument multiple times. Another way to do it, is to specify it from a file. Instead of _from-literal_ with specify _from-file_ and we pass the file path. Exmaple: `kubectl create configmap app-config --from-file=app_config.properties`.
- Declarative way: By creating a definition file. (see file _config-map.yaml_). Then we run the `kubectl create -f config-map.yaml` command. We can create as many as we want for different porpuses. For example, one for the database, one for redis. It is important to name them correctly since then we will associate them with the correpondant Pod. To view Config Maps we run the command `kubectl get configmaps` and `kubectl describe configmaps`

2. Inject them into the Pod.

We add a property called **envFrom** and **configMapRef**. (see file _pod-definition-2.yaml_)

### Secret

The _Config Map_ stores configuration data in plain text format. That is not a good idea in cases for example like a database password. Here is where _secrets_ come in. _Secrets_ are used to store sensitive information. In the secret the information is stored in an encoded format. There are two steps involved while working with _secrets_:

1. **Create the secret**

There are two ways:

- Imperative way byt passing directly from command line (`kubectl create secret generic <secret-name> --from-literal=<key1>=<value1> --from-literal=<key2>=<value2>`) or using a file (`kubectl create secret generic <secret-name> --from-file=<path-to-file>`)

- Declarative way (By creating a definition file). The values should be encoded like in base64, by doing in command line `echo -n 'mysql' | base64` (We echo the string and pipe the result to the base64 utility). To view the secret we use the commnad `kubectl get secrets` and `kubectl describe secrets`. This commands wont show the values. If we want to see the values we run `kubectl get secret <my-secret> -o yaml`.

2. **Inject it into the Pod**

The same way it is donde with _Config Maps_ we need to create a property called _envFrom_ and within it a property called _secretRef_ and in it a list, and each element of a list corresponds to a secret item (see file _pod-definition-3.yaml_)

#### Notes on Secrets

- Secrets are not encrypted, only encoded (anyone can decode it). Do not check-in Secret objects to SCM along with code
- Secrets are not encrypted in ETCD. So consider [encrypting at rest](https://kubernetes.io/docs/tasks/administer-cluster/encrypt-data/)
- Anyone able to create Pods/Deployments in the same namespace can access the secrets. For this configure least-privilege access to Secrets - RBAC.
- Consider thire-party secrets store providers, like AWS Provider, Azure Provider, GCP Provider, Vault Provider. There are other better ways of handling sensitive data like passwords in Kubernetes, such as using tools like Helm Secrets, HashiCorp Vault.
