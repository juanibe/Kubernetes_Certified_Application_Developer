# TAINTS AND TOLERATIONS

In Kubernetes, _taints_ and _tolerations_ are mechanisms used to control which nodes in a cluster can accept certain pods. They are particularly useful for scenarios where you want to restrict pods to run on specific nodes or to ensure that pods only run on nodes with certain characteristics.

They have nothing to do with security or intrusion on the cluster, they are used to **set restrictions on what pods can be scheduled on a node**

**Taints**

- A taint is a key-value pair that is applied to a node in a Kubernetes cluster.
- Taints are used to repel pods from being scheduled onto a node unless the pod has a corresponding toleration.
- Taints can have an effect of either "NoSchedule", "PreferNoSchedule", or "NoExecute".
  1. "NoSchedule" prevents new pods from being scheduled onto the node.
  2. "PreferNoSchedule" tries to avoid scheduling new pods onto the node but is not guaranteed.
  3. "NoExecute" evicts existing pods from the node that do not tolerate the taint.
- Taints are typically used to mark nodes with specific hardware capabilities, software configurations, or other properties that make them suitable only for certain types of workloads.

**Tolerations**

- A toleration is a pod-level attribute that allows a pod to be scheduled onto a node with a matching taint.
- When a pod has a toleration for a specific taint, it can be scheduled onto a node with that taint.
- Tolerations consist of a key, value, and effect, and they must match the corresponding taint on the node.
- Pods can have multiple tolerations, allowing them to be scheduled onto nodes with multiple matching taints.
- Tolerations are specified in the pod's YAML configuration.

### Creating Taints

We use the command `kubectl taint nodes <node-name> key=value:taint-effect` to taint a node. For example, if you would like to dedicate the node to Pods in application blue, then the key value pair would be _app=blue_. The _taint-effect_ defines what happens to the pods if they do not tolerate the taint. (See above first section). Example: `kubectl taint nodes node1 app=blue:NoSchedule`

### Adding tolerations

To add a toleration to a Pod, it is done in the _spec_ section of definition file called _tolerations_, and add the same values used while creating the _taint_ under this section. (see file _pod-definition.yaml_). All the values should be encoded between quotes. When the pods are now created or updated with the new tolerations they are either _not scheduled_ on nodes or _evicted_ from the existing nodes, depending on the event set.

### Clarifications

Taints and tolerations do not tell the Pod to go to certain nodes, instead they **tell the nodes to only accept certain pods**. If the requirement is to restrict the Pod to certain nodes, it is achieved with another concept called _node affinity_.

### Master node

When a Kubernetes cluster is first set up a taint is set on the master node automatically that prevents any pod to be scheduled on this node, it can be modified, however a best practice is not to deploy any pod on a master server. To see this tain run `kubectl describe node kubemaster | grep Taint`

### Removing a Taint

To remove a taint from a node in Kubernetes, you can use the kubectl taint command with the node keyword followed by the name of the node and the - (minus) symbol before the taint key. Here's the general syntax: `kubectl taint nodes <node_name> <taint_key>-`
