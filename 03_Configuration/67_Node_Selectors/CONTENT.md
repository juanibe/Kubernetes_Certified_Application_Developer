# NODE SELECTORS

Node selectors are a way to constrain the pods to only be able to run on nodes with particular labels. This is useful when you have different types of nodes in your cluster, and you want to ensure that certain pods run on specific nodes that meet certain requirements or have specific capabilities

Node selectors are defined in the pod's specification (spec) using the nodeSelector field. This field specifies a set of key-value pairs. When a pod is scheduled, Kubernetes looks for nodes that have labels matching the key-value pairs specified in the pod's _nodeSelector_, and it only considers those nodes as potential candidates for running the pod. (see file _pod-definition.yaml_).

### Node Labels

Before using node selectors in your pod specifications, you need to label the nodes in your Kubernetes cluster accordingly. Node labeling is a prerequisite for using node selectors effectively.

To label a node use the command `kubectl label node <node-name> <key>=<value>`. The labe is in a key value format. To match our example it would be: `kubectl label node node-1 size=Large`

When the Pod is then created (using our example file), it will be scheduled in "node-1" as desired.
