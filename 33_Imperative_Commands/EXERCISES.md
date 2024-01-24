- Deploy a pod named nginx-pod using the nginx:alpine image.
  `kubectl run nginx-pod --image=nginx:alpine`;

- Deploy a redis pod using the redis:alpine image with the labels set to tier=db. Either use imperative commands to create the pod with the labels. Or else use imperative commands to generate the pod definition file, then add the labels before creating the pod using the file.
  a. `kubectl run redis --image=redis:alpine --dry-run=client -o yaml > redis-pod.yaml`
  b. `kubectl create -f redis-pod.yaml`

- Create a service redis-service to expose the redis application within the cluster on port 6379. Use imperative commands.
  `kubectl expose pod redis --port=6379 --name redis-service`

- Create a deployment named webapp using the image kodekloud/webapp-color with 3 replicas. Try to use imperative commands only. Do not create definition files.
  `kubectl create deployment webapp --image=kodekloud/webapp-color --replicas=3`

- Create a new pod called custom-nginx using the nginx image and expose it on container port 8080.
  `kubectl run custom-nginx --image=nginx --port=8080`

- Create a new namespace called dev-ns. Use imperative commands.
  `kubectl create namespace dev-ns`

- Create a new deployment called redis-deploy in the dev-ns namespace with the redis image. It should have 2 replicas. Use imperative commands.
  `kubectl create deployment redis-deploy --namespace=dev-ns --image=redis --replicas=2`

- Create a pod called httpd using the image httpd:alpine in the default namespace. Next, create a service of type ClusterIP by the same name (httpd). The target port for the service should be 80. Try to do this with as few steps as possible.
  a. `kubectl run httpd --image=httpd:alpine`
  b. `kubectl expose pod httpd --port=80 --name httpd`
