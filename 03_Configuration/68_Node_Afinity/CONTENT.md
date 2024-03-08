# NODE AFFINITY

Node affinity is a Kubernetes feature that allows you to influence pod scheduling based on the properties of individual nodes in the cluster. It provides more sophisticated control over pod placement compared to simple node selectors.

It allows to specify rules that dictate how pods should be scheduled based on node labels. These rules can be defined using node affinity expressions, which consist of one or more node affinity terms. There are two types of node affinity terms:

- **requiredDuringSchedulingIgnoredDuringExecution**: Pods with this term in their node affinity expressions must be scheduled onto nodes that satisfy the specified rules during scheduling. However, if the node's labels change after the pod has been scheduled, the pod won't be evicted or moved.

- **preferredDuringSchedulingIgnoredDuringExecution**: Pods with this term in their node affinity expressions prefer to be scheduled onto nodes that satisfy the specified rules during scheduling. If no such nodes are available, the scheduler will still attempt to place the pod on nodes that do not satisfy the rules. Is a way to say to the scheduler _try your best to place the pod on matching node, but if you really can not find one, just place it anywhere_

**During scheduling** is the state where a pod does not exit and is created for the first time. **During execution** is the state where a pod has been running and a change is made in the environment that affects node affinity.

### How to define it

Under _spec_ we add the _affinity_ property and under it as a child _nodeAffinity_. Under this last one we add the _node affinity type_ (described above) and then _nodeSelectorTerms_ where we will define the _key-value_ pairs, that is an array. These key value paris are in for <key> <operator> <value> (see file _pod-definition.yaml_)

### Operators

Here are the common operators used in node affinity expressions:

- **In**: Specifies that the pod should be scheduled on a node if the node has a label with a value that matches any one of the specified values. For example, nodeSelector: key1: in: [value1, value2] means the pod can be scheduled on nodes labeled with key1 having either value1 or value2.

- **NotIn**: Specifies that the pod should not be scheduled on a node if the node has a label with a value that matches any one of the specified values. For example, nodeSelector: key1: notin: [value1, value2] means the pod cannot be scheduled on nodes labeled with key1 having either value1 or value2.

- **Exists**: Specifies that the pod should be scheduled on a node if the node has the specified label, regardless of its value. For example, nodeSelector: key1: exists means the pod can be scheduled on any node with the label key1, regardless of its value.

- **DoesNotExist**: Specifies that the pod should not be scheduled on a node if the node has the specified label. For example, nodeSelector: key1: doesnotexist means the pod cannot be scheduled on nodes with the label key1.

- **Gt (GreaterThan), Lt (LessThan), Gte (GreaterThanEqual), Lte (LessThanEqual)**: These operators are used for node affinity based on numeric values. They allow you to specify conditions such as greater than, less than, greater than or equal to, and less than or equal to. For example, nodeSelector: key1: gt: 5 means the pod can be scheduled on nodes with the label key1 whose value is greater than 5.
