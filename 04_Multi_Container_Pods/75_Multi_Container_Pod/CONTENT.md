# MULTI CONTAINER PODS

The idea of decoupling a large monolithic application into sub components known as microservices enables us to develop and deploy a set of independent, small, and reusable code. This architecture can then help us to scale up, down as well as modify each service as required as opposed to modifying the entire application. However at times you may need two services that target different functionalities to work toghether, but developed and deployed separately. For this reason is why we have **multi container pods** that share the same life cycle, which means they are created toghether and destroyed toghether. They share the same network space, which means they can refer to each other as _localhost_ and have access to the same storage volumes. This way you do not have to establish volume sharing or services between the pods to enable communication between them

### Create multi-container pod

Add the new container information to the pod definition file (see file _pod-definition.yaml_). As we can see, _containers_ section is an array, for which more than one container can be created.

### Design patterns

- **SIDECAR** The sidecar pattern in Kubernetes is a design pattern used to extend or enhance the functionality of a primary container by running additional containers alongside it within the same Kubernetes Pod. Common use cases can be logging, monitoring, security (certificate renewal, encryption), networking.
- **ADAPTER** Design pattern that involves using an additional container alongside the primary container within a Pod. However, unlike the sidecar pattern which provides supplementary functionalities to the primary container, the adapter pattern focuses on converting or adapting the interface of the primary container to meet the requirements of other systems or services. For example, in a logging service, we want to process the logs before sending them to the central server
- **AMBASSADOR** Involves using an additional container alongside the primary container within a Pod. However, unlike the Sidecar and Adapter patterns, the Ambassador pattern focuses on proxying and managing communication between the primary container and external systems or services.
