# DEPLOYMENTS

A deployment is a resource object that provides declarative updates to applications. It allows you to describe the desired state for your application, including which container images to use, the number of replicas, and how to update them over time. Deployments are a higher-level abstraction that simplifies the management and scaling of applications in a Kubernetes cluster

It provides us with the capability to upgrade the underlying instances seamlessly using rolling updates, undo changes, and pause andresume changes as required

### How to create a deployment

Same as other objects, we need to create a deployment deifinition file. Its contents are exactly similar to the replica set definition file, except for the _Kind_ which is now going to be **Deployment** (see file _deployment-definition.yaml_)

To create the deploymnet we need to run `kubectl create -f deployment-definition.yaml`

It will create a deployment (`kubectl get deployments`). Additionally, it will generate replica sets (`kubectl get deployments`) under the deployment's name. Furthermore, it will create pods (`kubectl get pods`) using the same name. Here, we can observe that the deployment holds a higher-level position. To view all the objects together, we can execute the `kubectl get all` command.
