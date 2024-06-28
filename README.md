## How to use our Docker image

### This is a quick guide on how to get the Docker image that contains the command-line software requested by the course instructors, and how to run it as a container on your system.

You can use it on your own computer to test programs and data you intend to use in your computer lab sessions. 

You will need a computer with an operating system (Host OS) able to run Docker:

https://docs.docker.com/engine/docker-overview/

---

### 1. Install Docker:

https://docs.docker.com/get-docker/

> [!NOTE]
> Remember that you may need to use ‘sudo’ for administrative commands if you are not logged in as root in your computer.


---

### 2. Get the docker image from our Docker Hub repository (you may need to prepend ‘sudo’ before any docker command):

The image has been primarily tested on Ubuntu Linux as Host OS.

`docker pull ppgcourseub/ppg2024:2406-r01`

This will take a while, depending on the speed of your internet connection.

After it's finished, you should see the downloaded image listed using this:

`docker images`

---

### 3. Run a container from the downloaded image (for the first time)

`docker run -it --name=container_name 9c539d570554 /bin/bash` 

> [!IMPORTANT]
> The string ‘9c539d570554’ is the “ID” of the image you got at step 1. **This ID may be different on your computer**.
> Check the correct ID on the listing from the `docker images` command. Also, the expression after '--name=' is
> the name of the new container. You can replace it with any other name you prefer.

After running the previous command, you will see a new prompt symbol on your terminal (starting by 'ppguser@' follower by an arbitrary ID string). You are now running a new Docker container (more exactly, an interactive shell session on this container). 

Think of a container as an instance of a Docker image. Your new container is running Ubuntu, and now you can run any of the installed programs on it.

You can exit your new running container at any time, mainly in two ways:
- Typing `exit`. This will also stop the container.
- Typing the sequence ctrl+p, ctrl+q. In this case the container will continue to run after you exit.

You can list all your containers with:

`docker ps -a`

Using this listing, you can always check if a container is currently running or stopped.

---

### 4. Re-connecting to your container:

If you stopped your container, first type:

`docker start container_name`

> [!NOTE]
> Remember to replace 'container_name' with the specific name of your container

And then type:

`docker exec -it container_name /bin/bash`

The command prompt of your container will appear again.

If your container is already running you can re-connect to it again with the previous command (docker exec ...) at any time.

> [!NOTE]
> There are other ways of re-attaching to your container when it's already running, this is just one of them (probably the simpler one)

---

### 5. Additional info: Running your container with  a 'shared' data directory

One of the most practical ways to make the data files you need to run your programs available to your container is to set a **shared directory** between your host OS and the container. All the data you put in that directory on your Host OS will be available also in the linked directory of your running container. Conversely, all the data you store in the linked container directory will be available in your Host OS corresponding directory. 

If you want to use this configuration, replace the initial 'docker run...' command from section 3 with this one:

* On Linux:

`docker run -it –name=container_name -v /home/username/ppgdata:/ppgdata 9c539d570554 /bin/bash`

* On Mac:

`docker run -it –name=container_name v /Users/username/ppgdata:/ppgdata 9c539d570554 /bin/bash`

...where '/home/username/ppgdata' or '/Users/username/ppgdata' is the host directory you want to 'share' with the conatiner (replace it with the directory you wish to use).

> [!IMPORTANT]
> The string ‘9c539d570554’ is the "ID" of the image you got at the step 1.
> **This ID may be different on your computer**. Check the ID on the listing from the ‘docker images’ command.

> [!IMPORTANT]
> Remember that the value after ‘--name=’ sets the name of the new running container. If you already created other container with the same name (from a previous docker run… command for example), you **will get an error**. To solve it, either change the name of the new container or remove (see below, section ‘Other useful commands’) the existing one with the conflicting name before creating the new one.

The difference here is the `-v` parameter, which creates a 'link' between the Host OS directory '/home/username/ppgdata' (or '/Users/username/ppgdata') and the container directory '/ppgdata'. To avoid permission problems inside your container when accessing the data in this directory, grant full read and write permissions on your Host OS directory (‘/home/username/ppgdata’ or 'Users/username/ppgdata' in this example) to all users. For example, on linux:

`chmod -R 777 /home/username/ppgdata`

After this, anything you put in the '/home/username/ppgdata' directory of your computer Host OS will be available immediately at the '/ppgdata' directory of your container, and also the other way around. This is a convenient and simple way of providing data files to use with your programs installed in your container, and also to get results and other data out from your container to your host OS.

This is the preferred way we will be using at the computer lab sessions to share data between the student’s computer OS and the containers running on them.

---

### Other useful commands:

To stop a container:

`docker stop container_name`
> [!NOTE]
> Remember: you can get the name of your container using `docker ps -a` 

To remove a container:

`docker rm container_name`

> [!WARNING]
> Note that a container needs to be stopped first to be able to remove it.

To remove images:

`docker rmi imageid`
> [!NOTE]
> Remember: you can get the id of your image using `docker images`


