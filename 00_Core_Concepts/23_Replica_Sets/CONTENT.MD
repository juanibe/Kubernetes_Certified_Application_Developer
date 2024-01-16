# REPLICA SETS

## Kubernetes controllers

Controllers are control loops that watch the state of your cluster, then make or request changes where needed. Each controller tries to move the current cluster state closer to the desired state

Here we will discuss one controller in particular and that is the replication controller

## What is a replica

Lets consider the scenario where we have a single POD running our app. In case our app crashes and the POD fails, then users won't be able to continue using the app.

To avoid this kind of situations we will want to be running more than one instance or POD running at the same time.

The REPLICATION CONTROLLER helps us running multiple instances of a single POD in the Kubernetes cluster thus providing high availability. But even in case you have a single POD, a replication controller can help by automatically bringing up a new POD when the existing one fails

## Load Balancing and Scaling

Another reason we need replication controller is to create multiple POD to share the load across them. While the number of users increase we deploy aditional POD to balance the load across all the users. If the demand further increases and if we were to run out of resources on the first node we could create additional PODS across other nodes in the cluster. As you can see the replication controller spans across multiple nodes in the cluster. It helps us balance the load across multiple PODS on different nodes as well as scale our application when the demand increases.

## Replica Sets and Replication controllers

They are similar and they have same purporse but they are not the same. Replication controller is the old technology that is being replaced by Replica Set. This last one is the new recommended way to set up replication.

## Creating a Replication Controller

- See file _rc-definition.yaml_

Like in any kubernetes definition file the spec section defines what's inside the object we are creating. In this case we know that the replication controller creates multiple instances of a POD. But what POD?. For that we create a _template_ section to provide a POD template to be used by the replication controller to create replicas. To create the template we use the same as if we were creating a pod (see file _15_Creating_Pods_With_Yaml/pod-definition.yaml_) except for the values _apiVersion_ and _kind_

Now we have two metadata sections, one if for the Replication Controller and the other for the POD, and same for two spec sections.

We haven't specified yet how many replicas we need in the replication controller. For that we use in the spec section (of the main file) the property _replicas_

## Running

- To run the file we use _kubectl create -f rc-definition.yaml_

- To see the replication controller we use _kubectl get replicationcontroller_

## Creating a Replica Set

- See file _replicaset-definition.yaml_

It is very similar to Replication Controller. Changes the _apiVersion_ and _kind_ properties. However there is an important difference with Replication Controller. Replica Sets require a _selector_ definition. It goes under the main _spec_ property and it helps the replica set identify what PODS fall under it. But why do we have to specify what PODS fall under it if we have provided the contents of the POD definition file itself in the template section?. It is because Replica Sets can also manage PODS that were not created as part of the Replica Set creation. Say for example, there were PODS created before the creation of the replica set that match labels specified in the selector, the Replica will also take those PODS into consideration when creating the Replicas.

In the Repliaction Controller the _selector_ property exists but is not required. On the other hand, when using Replica Set it is required.

### Match labels

Under the _selector_ property we use the match labels and the values labels **has to match the values in the labels in the pod we are trying to match**

### Labels and Selectors

Why do we label and select our objects in Kubernetes?. Say we deploy three instances of our frontend web application as three pods. We would like to create a Replication Controller or Replica Set to ensure that we have three active Pods at any time. And yes, that is one of the use case of replica sets. You can use it to monitor existing pods if you have them already created. In case they were not created the replica set will create them for you. The role of the Replica Set is to monitor the PODS and if any of them were to fail, deploy new ones. The Replica Set is in fact a process that monitor the PODS. Now how does the Replica Set know what POD to monitor?. There could be hundred of other PODS running different applications in the cluster. And this is ehre labeling our PODS during creation comes in handy. We can provide these labels as a filter for the Replica Set

### Scale Replica Set

There are multiple ways to scale a replica set:

- The first is to update the number of replicas in the definition file. Then run `kubectl replace -f definition-file.yaml` which will update the replica set (specifying the same file).
- Use the `kubectl scale --replicas=<number-of-replicas> -f definition-file.yaml` command (specifying the same file). Note that the number specified in the file itself will continue being the same as it was.
- There are also optins to scale the replica set based on load which will be discussed at a later time.
