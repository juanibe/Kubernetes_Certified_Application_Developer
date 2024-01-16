# PODS

## Concept

In Kubernetes, a "pod" is the smallest and simplest unit in the Kubernetes object model. It represents a single instance of a running process in a cluster and encapsulates one or more containers. Containers within a pod share the same network namespace, allowing them to communicate with each other using localhost.

Here are some key points about pods:

- Basic Unit: Pods are the basic building blocks in Kubernetes. They can contain one or more containers that are tightly coupled and share the same lifecycle.

- Co-located Containers: Containers within a pod share the same network and storage, making it easier for them to communicate and share data. They can also use inter-process communication mechanisms like local IPC.

- Atomic Unit: Pods are considered as the smallest deployable units in Kubernetes. If you need to scale your application, you typically scale the number of pods.

- Pod Lifecycle: Pods have a defined lifecycle, including a creation phase, a running phase, and a termination phase. When a pod is terminated, all the containers within it are terminated as well.

- Shared Resources: Containers within the same pod share the same IP address and port space. They can communicate with each other using localhost, which simplifies networking.

- Pods are typically used to deploy applications in Kubernetes. However, in many cases, it's common to have pods with a single container. Pods are managed and scheduled by the Kubernetes control plane, and they can be horizontally scaled to meet application demands.

## How to run a POD

- Using `kubectl` command:
  `kubectl create -f pod-definition.yaml`

- To see the running pods:
  `kubectl get pods`
