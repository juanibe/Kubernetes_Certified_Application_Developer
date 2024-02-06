# SERVICE ACCOUNTS

[See](https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/)

The concept of _Service Accounts_ is related to other security conecpts such as _authentication_, _authorization_, _roles_, but since for the _Application Developer Exam Curriculum_ are not necessary, we won't add it here and we only need to know how to work with **Service Accounts**

### Account Types:

There are two types of accounts in _Kubernetes_:

- **User Account**: Used by humans, such as an administrator, or a developer to deploy an application.
- **Service Accounts**: Used by machines. Could be an account used by an application to interact with the Kubernetes cluster. For example, a monitoring application, like _Prometheus_ uses a service account to pull the Kubernetes API for performance metrics. Also, an automated tool like _Jenkins_ uses a service account to deploy applications on the kubernetes cluster

### Creating Service Accounts and Tokens

To create a service account we run the command `kubectl create serviceaccount <name>`. To get a token for that service account, we can run the command `kubectl create token <name>`. The output from that command is a token that you can use to authenticate as that ServiceAccount. You can request a specific token duration using the `--duration` command line argument to _kubectl create token_ (the actual duration of the issued token might be shorter, or could even be longer).

### What if the third party application is hosted on the kubernetes cluster itself?

For example, we can have a kubernetes dashboard custom application. In this case, the whole process of exporting the service account token and configuring the third party application to use it by automatically mounting the service token Secret as a volume inside the Pod hosting the third party application. This way, the token to access the Kubernetes API is already placed inside the Pod and can be easily read by the application (it doesn't need to be provided manually)

### Injecting and editing the Service Account.

If we don't want to use the deafult service account, we can inject one created by us. (see file _pod-definition.yaml_). It is not possible to edit the service account in a Pod. It has to be done by deleting and recreating the Pod. However in case of a _Deployment_ you will be able to edit the service account, since any changes to the _pod definition file_ will automatically trigger a new rollout for the deployment (the deployment will take care or deleting and recreating new pods)

### Steps:

- Create service account (`kubectl create serviceaccount myserviceaccount`)
- Create a token (`kubectl create token myserviceaccount`)
