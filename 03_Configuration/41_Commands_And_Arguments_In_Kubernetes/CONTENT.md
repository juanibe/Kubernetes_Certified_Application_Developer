# COMMANDS AND ARGUMENTS IN KUBERNETES

We will use the _Dockerfile_ created in section _40_ to create a _POD_. Since that file requires an argument (the seconds the Ubuntu OS will sleep) we need to pass it in the _definition file_. That will go in the **args** property within containers withing spec in the form of an array. (see file _pod-definition.yml_). Remeber that the _ENTRYPOINT_ is the command that is run at startup and _CMD_ is the default argument that will be added if no other one is passed. Since here we are passing the arguments, then it will be overwritten.

**Overwritting the entrypoint**: For example, if we would like to run _sleep-2.0_ instead of _sleep_. For that we need to specify at the same level of _args_ the property **command**. It is also an array.
