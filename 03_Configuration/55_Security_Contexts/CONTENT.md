# SECURITY CONTEXT

As we saw in module 54 when you run a docker containe you have the option to define a set of security standards, such as the id of the suer used to run the container or linux capabilities. These, can be configured in Kubernetes as well.

As we know, in Kubernetes containers are encapsulated in Pods. You may choose to configure the security settings at a container level or at a Pod level. If it is done at a Pod level the settings will carry-over to all the containers within the Pod. If is is done in both, at container level and Pod level, the setting in the container will override the settings on the Pod.

To configure the security at Pod level we need to add a section _securityContext_ under the _spec_ section of the Pod. (see file _pod-definition.yaml_). In the example we see that we use the _runAsUser_ option to specify which user will run the process. But if we want to do it at container level, we move the section under the _containers_ section (see file _pod-definition-container-level.yaml_). Since capabilities are only supported at container level, in the example we can see that we add capabilities.
