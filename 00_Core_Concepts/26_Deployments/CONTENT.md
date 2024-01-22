# DEPLOYMENTS

A deployment is a resource object that provides declarative updates to applications. It allows you to describe the desired state for your application, including which container images to use, the number of replicas, and how to update them over time. Deployments are a higher-level abstraction that simplifies the management and scaling of applications in a Kubernetes cluster

It provides us with the capability to upgrade the underlying instances seamlessly using rolling updates, undo changes, and pause andresume changes as required

### How to create a deployment

Same as other objects, we need to create a deployment deifinition file. Its contents are exactly similar to the replica set definition file, except for the _Kind_ which is now going to be **Deployment** (see file _deployment-definition.yaml_)

To create the deploymnet we need to run `kubectl create -f deployment-definition.yaml`

It will create a deployment (`kubectl get deployments`). Alos it will create replica sets (`kubectl get deployments`) in the name of the deployment. And also, it will create pods (`kubectl get pods`) in its name. There we can see that the deployment is in the higher place. To see all the objects toghether we can run the `kubectl get all` command
