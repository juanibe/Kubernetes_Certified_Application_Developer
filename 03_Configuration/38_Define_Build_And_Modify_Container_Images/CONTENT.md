# CONTAINER IMAGES

### Why do we need to create container images?

It could be either because you can't find a component or a service that you want as part of your application on Docker Hub already or you and your team decided that the application you are developing will be dockerized for ease of shipping and deployment

### How to create a Docker Image

- Write a file called Dockerfile
- Write the instructions for setting up your application. This is the same as if you would be deploying manually you application, such us installing dependencies, where to copy the source code from and to and what the entry point of the application is, etc...
- Build the image with the _docker build_ command and specify the docker file as input: `docker build -t <app-name>`
- That will create the image in your local system. To make it available in the Docker Hub registry we run the _docker push_ command and specify the name of the image you just created: `docker push <app-name>`

### The docker file

Is a text file written in a specific format that Docker can understand. It's in an instruction and arguments format. In the file _Dockerfile_ everything on the left in caps in an instruction. Each of them instruct Docker to perfrom a specific action while creating the image. Everything on the right is an argument to those instructions.

- The first line _FROM Ubuntu_ defines what the base OS should be for this container. Start from a base O.S or another image (which will start from a base O.S). Every Docker image must be based off of another image. You can find official releases of all O.S on Docker Hub. All Docker Files **must** start with the _FROM_ instruction

- The _RUN_ instruction instructs Docker to run a particular command on those base images. For example, example install dependencies.
- The _COPY_ instruction copies files from the local system onto the Docker image. In our example, copy the code in the current location to the location inside the dokcer image _opt/source-code_
- The _ENTRYPOINT_ allows us to specify a command that will be run when the image will be run as a container.

### Layered architecture

When Docker builds the images it builds them in a **layered architecture**. Each line of instruction creates a new layer in the Docker Image with just the changes from the previous layer. In our example, the first layer is the based image Ubuntu O.S followed by the second instruction that creates a second layer which installs all the _apt_ packages and then the third instruction creates a third layer with the python packagesm, then a fourth layer that copies the source code and the last one that updates the entry point.

Since each layer only stores the changes from the previous layer it is reflected in the size as well. We can see this information if we run the docker history command followd by thei image name: `docker history <image-name>`

### Docker build output

When you run the _docker build_ command you can see the stepts involved and the result of each task. All the layers built are cast so the layered architecture helps you restart docker build from that particular step in case it fails. Or if you were to add new steps in the build process, it wouldn't have to start all over again

### Failure

If one step fails, Docker will reuse the previous layers from cache and continue to build the remaining layers. The same is true if you were to add additional steps in the Docker file. This is helpful when you update source code of your application, as it may change more frequently.

### What can you containerize?

You can containerize almost all of the application, even simple ones like browsers, or utilities like curl, applications like spotify, skype, etc. Basically you can containerize everything.
