# COMMANDS AND ARGUMENTS IN DOCKER

Say we want to run a Docker container from an Ubuntu image. When you run `docker run ubuntu` it runs an instance of Ubuntu and exits immediately. If you list the running containers you will not see the container running. But if you list the containers that are also stopped, you will see that the container will be in an `exit` state.
Why?. Unlike VM's containers are not meant to host and O.S, but to run an specific task or process, such as to host an instance of a web server, or a database for example. The container only lives as long as the process inside it is alive.

### Who decides what process is run within a container?

If we take a look at popular docker images like nginx, we will see an instruction called `CMD` that stands for command, that defines the program that will be run within the container when it starts. For the nginx image the command is _nginx_.
What we tried to do earlier was to run an Ubuntu image that uses `bash` as the default command. But `bash` is not really a process like a web server or database server. It is a shell that listens for inputs from a terminal and if it cannot find a terminal it exits. By default docker does not attach a terminal to a container when it is run and so the Bash program does not find the terminal and so it exits, and the container exits aswell.

### How do you specify a different command to start the container?

One option is to append a command to the _docker run_ command, and that way it overrides the default command specified within the image. For example `docker run ubuntu sleep 5`. This way it will wait for five seconds and then it exists. But how do we make that change permanent?. For that we will create our own image from teh base Ubuntu image and specify a new command. (see file _Dockerfile_). It can be written in different ways: - Like if it were done from the command line, or in json array format. In this last case, the first element.should be the executable always. The command and arguments should be separated elements in the list.

### How could we pass the arguments dynamically?

In our example, the argument `5` is hardcoded. Let's say we want to pass in the number of seconds Ubuntu will sleep. Here is where the _ENTRYPOINT_ instruction comes into play. The _ENTRYPOINT_ instruction is like the _CMD_ instruction, as in you can specify the program that will be run when the container starts. And whatever you specify in the command line will be appended to the _ENTRYPOINT_ command. So for instance, if we run `docker run ubuntu-sleeper 10` and our _ENTRYPOINT_ in the docker file is `ENTRYPOINT ["sleep"]` we will be running `ENTRYPOINT ["sleep", "10"]`
