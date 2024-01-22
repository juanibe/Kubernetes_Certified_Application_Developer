# NAMESPACES

### Concept

In programming, a namespace is a container that holds a set of identifiers, such as variables, functions, classes, or other symbols, and provides a way to organize and isolate them to avoid naming conflicts. This helps in creating modular and organized code by preventing naming collisions between different parts of a program or between different libraries.

In the context of Kubernetes, namespaces are used to create virtual clusters within a physical cluster. Kubernetes namespaces are a way to divide cluster resources between multiple users or projects. They provide a scope for names, and each resource (such as pods, services, or volumes) in a Kubernetes cluster must be associated with a namespace.
Using namespaces in Kubernetes allows you to isolate and organize different applications or environments within the same cluster. This helps prevent naming conflicts and provides a level of resource isolation. For example, you might have separate namespaces for development, testing, and production environments within a Kubernetes cluster.

So far we've created objects such as Pods, Deployments and Services in our cluster. Whatever we have been doing, we have been doing within a **namespace**. This namespace is the default one and is created by kuberenetes

Kubernetes also creates a set of Pods and Services for its internat purpose such as those required by the networking solution, the DNS service, etc... To isolate this from the user and to prevent you from accidentally deleting of modifying these services kuberentes creates them under another _namespace_ created at cluster startup named _kube-system_
Another namespace created by kubernetes automatically is called _kube-public_. This is where resources should be made available to all users are created. If your environment is small, or you are learning or playing around with a small cluster, you shouldn't really worry about namespaces and you can continue to work in the default namespace. However, as you grow and you use kubernetes for enterprise or production purporses you may want to consider the use of namespaces.

### Isolation

You can create your own namespace. For example if you want to use the same cluster for both develop and production environment, but at the same time, isolate the resources between them, you can create a different namespace for each of them. That way when working on develop environment you dont accidentally modify resources in production

### Policies

Each of these namespaces can have its own set of policies that define who can do what. You can also assign quota of resources to each of these namespaces. That way each namespace is guaranteed a certain amount and does not use more than its allowed limit

### DNS

The resources within a namespace can refer to each other simply by their names. If required a resource can access to another one that is in another namespace. To do that we must append the name of the namespace to the name of the service. Example:

`mysql.connect("db-service.dev.svc.cluster.local")`

You're able to do this because when the service is created a DNS entry is added automatically in that format. Looking closely to the last part, `cluster.local`, is the default domain name of the kubernetes cluster, `svc` is the subdomain for service, followed by the namespace `dev` and then the same of the service itself, in this example, `db-service`

### Operational aspects

**Kubectl commands**

- Command to list all namespaces: `kubectl get namespaces` or `kubectl get ns`

- Command to list all the PODS in the _default_ namespace.`kubectl get pods`. To list Pods in another namespace, use the _namespace_ option in the command along with the name of the namespace `kubectl get pods --namespace=kube-system` or `kubectl get pods -n=kube-system`

- Command to create a pod, same as the case of listing, we can use the _namespace_ option to specify an specific namespace. `kubectl create -f pod-definition.yaml --namespace=dev`. If we want that the Pod is always created in that namespace even if we don't specify it in the command line, we can move namespace definition file. (see file _pod-deifinition.yaml_)
  This is a good way to ensure your resources are always created in the same namespace

### Creating namespaces

As every resource in kubernetes is done by creating a difinition file. (see file _namespace-dev.yaml_).
Another way to create a namespace is by simply running the command `kubectl create namespace namespace-dev`

### Switch

Say we are working in three namespaces. We will always see the default one unless we specify by command line which one to see. Instead of doing that, if we want to always work with the, for example, develop namespace, we can run this command so we don't have to specify it each time we run a command.

`kubectl config set-context $(kubctl config current-context) --namespace=dev`

After that we can simply run the `kubectl get pods` and will return the pods belonging to the develop namespace, without having to specify it in the command line

In case we want to see the Pods in all namespaces we can use the option _all-namespaces_ option int the command line

`kubectl get pods --all-namespaces` or `kubectl get pods -A`

### Resource Quota

To limit resources in a namespace, create a resource quota. To create one we do it by creating a definition file. (see file _compute-quota.yaml_). To create it we run `kubectl create -f compute-quota.yaml`
