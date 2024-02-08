# RESOURCE REQUIREMENTS

In Kubernetes, managing resource requirements is crucial for effective workload scheduling, optimization, and ensuring the stability and performance of your applications. Here are several important aspects to consider regarding resource requirements in Kubernetes:

- Pod Resource Requests and Limits: When deploying applications in Kubernetes, you can specify the CPU and memory resource requirements for each pod. Resource requests indicate the minimum amount of CPU and memory that a pod needs to run, while resource limits define the maximum amount of resources that a pod can consume. These specifications help Kubernetes scheduler to make appropriate decisions regarding pod placement and ensure that pods have adequate resources to function properly.

- Resource QoS (Quality of Service): Kubernetes categorizes pods into three Quality of Service classes based on their resource requests and limits:

1.  Guaranteed: Pods with both CPU and memory requests and limits specified. They are guaranteed to have the specified resources available.
2.  Burstable: Pods with CPU and/or memory requests but no limits. They can use more resources if available, up to the specified limits.
3.  BestEffort: Pods with no resource requests or limits. They can use any available resources in the node.

- Resource Allocation and Utilization Monitoring: Kubernetes provides monitoring tools like Metrics Server, which collects resource usage metrics from pods and nodes. This data is essential for monitoring resource utilization, identifying potential bottlenecks, and making informed decisions about resource allocation and scaling.

- Autoscaling: Kubernetes supports horizontal pod autoscaling (HPA), which automatically adjusts the number of pod replicas based on resource utilization metrics such as CPU or memory usage. Autoscaling helps to maintain optimal resource utilization and ensure that applications can handle varying workloads efficiently.

- Resource Quotas and Limits: Kubernetes allows administrators to set resource quotas at the namespace level to limit the total amount of CPU, memory, and other resources that can be consumed by pods within that namespace. This helps in resource governance and prevents individual workloads from monopolizing cluster resources.

- Node Scheduling and Resource Availability: Kubernetes scheduler considers node resource availability (CPU, memory) when placing pods onto nodes. Nodes with sufficient available resources are selected for pod placement to ensure that pods can be scheduled without resource contention.

- Resource Management Policies: Kubernetes allows you to define resource management policies using features like Pod Priority and Preemption, which enable you to prioritize certain pods over others based on factors like resource requirements and application criticality.

### Resource Requests

It can be specified the amount of CPU and memory required for a Pod when creating one. For example it could be 1 CPU and 1 GB of memory. So Resource Request is the minimum amount of CPU or memory requested by the container, so when the scheduler tries to place the Pod on a Node, it uses these numbers to identify a node which has sufficient amount of resources available (see file _pod-definition.yaml_ for an example of how to do it)

### CPU - What does 'One count of CPU' really mean?

You can specify _0.1_ or _100m_ for example. And you can go as low as _1m_ but not lower than that.
One count of CPU is the equivalent to one **vCPU** which is one CPU in _AWS_ or 1 **Azure Core** in _Azure_ or 1 **GCP** in Google Cloud.

### Memory

You can specify for instance 256 **Mi** (_mebibyte_) or as **M** for megabytes or **G** for Gigabyte.

### Resource Limits

You can set a limit for the resource usage on the Pods that don't specify the amount. This can be done with CPU and Memory (see file _pod-definition-limits.yaml_)

### Exceed Limits

In the case of the CPU the system throttles the CPU so it does not go beyond the specified limit. A container can not use more CPU resources than its limit. However this is not the case with memory. A container can use more memory resources than its limits. But if a Pod consumes more memory resources than its limits **constantly** then the Pod will be terminated with an **OOM (Out of memory)** error.

### Default configuration

By deafult kuberentes does not have a CPU or memory request or limit set. This means that any pod can consume as much resources as required on any node and suffocate other pods or processes that are running on the node

### Limit Ranges

With limit ranges we can ensure that a every Pod is created with a default set. It is created with a definition file (see file _limit-range-cpu.yaml_)

### Resource Quotas

Resource Quotas in Kubernetes are a feature that allows administrators to limit the amount of compute resources (such as CPU and memory) that can be consumed by Pods, Containers, and other Kubernetes objects within a namespace. This helps in ensuring fair resource allocation and prevents any single user or application from monopolizing the cluster's resources, which could lead to performance degradation or even cluster instability. (See file _resource-quota.yaml_)
