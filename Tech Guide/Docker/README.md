# Docker Essentials

- [Docker Essentials](#docker-essentials)
  - [Part 01 (Introduction)](#part-01-introduction)
  - [Part 02 (Docker Overview)](#part-02-docker-overview)
    - [Chapter 1 (Docker on Linux)](#chapter-1-docker-on-linux)
    - [Chapter 2 (Docker on Windows)](#chapter-2-docker-on-windows)
    - [Chapter 3 (Docker on Mac)](#chapter-3-docker-on-mac)
  - [Part 03 (Commands)](#part-03-commands)
  - [Part 04 (Run)](#part-04-run)
  - [Part 05 (Environment Variables)](#part-05-environment-variables)
  - [Part 06 (Create an Image Using Commit)](#part-06-Create-an-Image-Using-Commit)
  - [Part 07 (Create an Image Using Dockerfile)](#part-07-Create-an-Image-Using-Dockerfile)
  - [Part 08 (CMD vs ENTRYPOINT)](#part-08-cmd-vs-entrypoint)
  - [Part 09 (Networking)](#part-08-networking)
  - [Part 10 (Storage)](#part-10-storage)
  - [Part 11 (Compose)](#part-11-compose)
  - [Part 12 (Registry)](#part-12-registry)
  - [Part 13 (Engine)](#part-13-engine)
  - [Part 14 (Docker Orchestration)](#part-14-docker-orchestration)
    - [Chapter 1 (Docker Swarm)](#chapter-1-docker-swarm)
    - [Chapter 2 (Kubernetes)](#chapter-2-kubernetes)
  - [Part 15 (Conclusion)](#part-15-conclusion)
  - [Part 16 (Terminology)](#part-16-Terminology)
  - [Part 17 (References)](#part-17-references)

## Part 01 (Introduction)
Why do you need Docker?
- You don't need to worry about Compatibility/Dependency
- Short Setup Time/Dependency Resolvement
- You can easily work with different Dev./Test/Prod. environments
- Lower computation overhead compare to the traditional virtualization methods

What can it do?
- Containerize Applications
- Run each service with its own dependencies in seperate containers

What are containers?
- Containers are completely isolated environments. They can have their own processes for services, network interface, mounts just like VMs, except they all share the same OS kernel.
- Containers are not introduced with Docker. They existed for more than 10 years. For instance, LXC, LXD, LXCFS, etc.
- Docker utilizes [LXC containers](https://en.wikipedia.org/wiki/LXC).

With docker you'll be able to run each component in a seperate container with its own dependencies and its own libraries. All on same VM/OS but within seperate env./container. -> build docker configuration once

Public Docker Repository = [DockerHub](https://hub.docker.com)

How is it done?
- `$ docker run IMAGE_NAME`
- You can run images as many instances as you want. (For load balancing, backup or alternative, ...)

Container vs Image:
- Containers are running instances of images that are isolated and have their env. and sets of processes.
- Images (Package + Template + Plan) -> It is used to create one or more containers

## Part 02 (Docker Overview)
Docker Editions:
- Community (Free)
  - Linux
  - Mac
  - Windows
  - Cloud (AWS, AZURE, ...)
- Enterprise -> Certified and Supported container platform
  - Enterprise Add-ons: Image Management + Image Security + ...

### Chapter 1 (Docker on Linux)
1. Go to [Docker Documentation](https://docs.docker.com) and click on [Get Docker](https://docs.docker.com/install/)
2. From sidebar menu select your OS/Distribution (_In my case, I use Ubuntu 18.04_)
3. Make sure to uninstall older versions:
   - If you want to keep configuration file of older version:
      ```bash
      sudo apt remove docker docker-engine docker.io containerd runc
      ```
   - If you want to delete everything from your system:
      ```bash
      sudo apt purge docker docker-engine docker.io containerd runc
      ```
4. Scroll down to _Install using the convenience script_
    ```bash
    curl -fsSL https://get.docker.com -o get-docker.sh
    sudo sh get-docker.sh
    sudo docker version
    ```
5. If you would like to use Docker as a non-root user, you should now consider adding your user to the “docker” group with: `$ sudo usermod -aG docker $USER`

### Chapter 2 (Docker on Windows)

- You won't be able to run Windows-based container on Docker host with Linux on it. For that you'll require a docker on a Windows Server.
- Because they don't have a same kernel.
- Here's a little trick that Windows do to run a Linux-based container in Docker:
  - When you install docker on Windows and run a Linux container on Windows, you're not really running a Linux container on Windows. Windows runs a Linux container on a Linux virtual machine (VM) under the hood. So it's really Linux container on a Linux virtual machine on Windows.

We can use Docker on Windows with these 2 options to run a Linux container on a Windows host:
1. Docker on Windows using Docker Toolbox (Original support for docker on Windows)
   - System Requirement:
     - 64-bit operating system
     - Windows 7 or Higher
     - Enable Virtualization
   - Docker Toolbox:
     - Oracle VirtualBox
     - Docker Engine
     - Docker Machine
     - Docker Compose
     - Kitematic GUI -> (It's a user interface)
2. Docker Desktop for Windows
   - It uses _Hyper-V_ instead of _Virtualbox_
   - It only works on these system, because they support _Hyper-V_ by default:
     - Windows 10 Professional Edition
     - Windows 10 Enterprise Edition
     - Windows Server 2016
     - Nano Server
- VirtualBox and Hyper-V cannot coexist on the same Windows host. So if you started with docker toolbox with VirtualBox and if you plan to migrate to Hyper-V, remember you cannot have both solutions at the same time. There is a migration guide available on docker documentation page on [how to migrate from VirtualBox to Hyper-V](https://docs.docker.com/docker-for-windows/docker-toolbox/).
- When you install docker desktop for Windows, the default option is to work with Linux containers. But if you would like to run Windows containers then you must **explicitly** confiure docker for Windows to switch to using Windows containers.
- Microsoft announced Windows Server 2016 support Windows containers for the first time. You can now packaged applications, Windows applications into Windows docker containers and run them on Windows docker host using docker desktop for windows. Now you can create Windows-based images and run Windows containers on a Windows server just like how you would run Linux containers on a Linux system.
- Unlike in Linux there are two types of containers in Windows:
  1. Windows Server container which works exactly like Linux containers where the OS kernel is shared with the underlying operating system to allow better security boundary between containers and to a lot of kernels with different versions and configurations to coexist.
  2. The Hyper-V Isolation: With Hyper-V isolation each container is run within a highly optimized virtual machine guaranteeing complete kernel isolation between the containers and the underlying host. While in the Linux world you had a number of base images for a Linux system such as Ubuntu, Debian, Fedora, Alpine, etc. if you remember, that is what you specify at the beginning of the dockerfile. In the Windows world we have two options (Base Images):
     1. Windows Server Core
     2. Nano Server
- Nano Server is a headless deployment option for Windows Server which runs at a fraction of size of the full operating system. (Like Alpine image in Linux)
- Windows Server Core is not as light weight as you might expect it to be.

### Chapter 3 (Docker on Mac)
1. Docker on Mac using Docker Toolbox (Original support for docker on Mac)
   - It's a docker on a Linux VM created using VirtualBox on Mac as with Windows, it has nothing to do with Mac applications or Mac based images or Mac containers. It purely runs Linux containers on MacOS.
   - System Requirements:
     - macOS 10.8 "Mountain Lion" or newer
   - Docker Toolbox:
     - Oracle VirtualBox
     - Docker Engine
     - Docker Machine
     - Docker Compose
     - Kitematic GUI -> (It's a user interface)
2. Docker Desktop for Mac
   - It uses _HyperKit_ instead of _Virtualbox_
   - It will still automatically create a Linux system underneath and it's created on HyperKit.
   - It only works on these system:
     - macOS Sierra 10.12 or newer
     - Mac Hardware 2010 model or newer
- Remember, that all of this is to be able to run Linux container on Mac.
- Up to this point, there are no Mac-based images or containers.

## Part 03 (Commands)
- Run = Start a container or run a container from an image. If the image is not present on the host, it will go to _dockerhub_ and pull the image down. (Only the first time) Container will run in fg (foreground) or technically in _attach mode_.
```
docker run SERVICE
```
- List of containers with basic informations. If you want to see the list of all containers (availabe, stopped and exited) use `-a`
```
docker ps
docker ps -a
```
- Stop a container using container ID or NAME
```
docker stop ID/NAME
```
- Remove a container using ID or NAME only for stopped or exited containers
```
docker rm ID/NAME
```
- List of images and their sizes
```
docker images
```
- Remove an image. You must ensure that no containers are running off of that image before attempting to remove the image. You must stop and delete all dependant containers to be able to delete an image.
```
docker rmi NAME
```
- Pull/Download an image when it couldn't find one locally. It only pulls an image without running it.
```
docker pull NAME
```
**Unlike VMs, containers are not meant to host an OS (operating system). Containers are meant to run a specific task or process such as to host an instance of a web server or an application server or a database or simply to carry some kind of computation or analysis tasks, once the task is complete, the container exits. The container only lives as long as the process inside it is alive.**
- Append a command (Execute a command when we run the container)
```
docker run SERVICE COMMAND
docker run ubuntu sleep 5
```
- Execute a command (for a running container)
```
docker exec NAME/ID COMMAND
```
- Run a container (Attach and Detach) -> Detach means don't show output of command and work in bg (background) using `-d`. Using attach command will bring your output to fg (foreground)
```
docker run -d SERVICE
docker attach NAME/ID
```

## Part 04 (Run)
- Run with specific version/tag. Default tag will be latest version
```
docker run SERVICE:TAG
docker run redis:4.0
```
- Run with STDIN (Standard Input). Docker container doesn't listen to STDIN even though you are attached to its console. If we want to run a container on interactive mode, we should use `-i` (interactive). However, we can't see the application's prompt on the terminal because we haven't attached to the container's terminal. To solve this, we should use `-t` (terminal).
```
docker run -it SERVICE:TAG
```
- Remove Container After Usage. Sometimes, it’s useful just to start a container to poke around, and then discard it afterward. The following will start a new container, drop into a shell, and then destroy the container after you exit.
```
docker run --rm -it  SERVICE:TAG
```
- Port Mapping / Port Publishing. Every docker container gets an IP assigned by default. Docker container IP is 172.17.1.2:3000 and Docker host IP is 192.168.1.25 so, you can map their port like below. You can even run multiple instances of your application and map them to different ports on the docker host or you can run your application on a single port and map them to different port. Remember, you cannot map to the same port on the docker host more than once.
```
docker run -p USER_PORT:CONTAINER_PORT SERVICE
docker run -p 80:3000 redis
docker run -p 443:3001 redis
docker run -p 8080:3001 redis
```
- Volume Mapping. The docker container has its own isolated filesystem and any changes to any files happen within the container. Sometimes we need a persistent data, so we need o map a directory outside the container on the docker host to a directory inside the container. We use `-v` (Volume) option. `DIR_HOST` is a directory outside docker host which docker mount it inside docker host which we call it `DIR_CONTAINER`.
```
docker run -v DIR_HOST:DIR_CONTAINER SERVICE
docker run -v /opt/sql/data:/var/lib/mysql mysql
```
- Inspect Container. If you would like to see additional details about a specific container use inspect command. It will give you more information than `docker ps` command. This command will give you all details of a container in a JSON format.
```
docker inspect NAME/ID
```
- Container Logs. See logs of a container runs in the background (detached mode).
```
docker logs NAME/ID
```

## Part 05 (Environment Variables)
- An environment variable is a variable whose value is set outside the program, typically through functionality built into the operating system or microservice. An environment variable is made up of a name/value pair, and any number may be created and available for reference at a point in time.
- Assignments:
  - OS
    - Unix (Terminal): `export VARIABLE=value`
    - Windows (Command Prompt or cmd): `SET VARIABLE=value`
  - Programmming Languages
    - Python3: `var = os.environ.get('VARIABLE') = value`
    - C++: `DATATYPE VARIABLE=value; putenv(VARIABLE);`

```
docker run -e VARIABLE=value NAME/ID
```
- To deploy multiple containers with different Environment Variable, you should run docker command multiple times and set different Environment Variables each time.
- If you want to fine the environment variable set on a running container, you should simply use `docker inspect NAME/ID` and look for `ENV` under `Config` section.

## Part 06 (Create an Image Using Commit)

Let’s start by running an interactive shell in a ubuntu container:

```
docker container run -ti ubuntu bash
```
To customize things a little bit we will install a package called [figlet](figlet.org) in this container.

```
apt-get update
apt-get install -y figlet
figlet "hello docker"
```

You should see the words “hello docker” printed out in large ascii characters on the screen.

Now let us pretend this new figlet application is quite useful and you want to share it with the rest of your team. You could tell them to do exactly what you did above and install figlet in to their own container, which is simple enough in this example. But if this was a real world application where you had just installed several packages and run through a number of configuration steps the process could get cumbersome and become quite error prone. Instead, it would be easier to create an image you can share with your team.

To start, we need to get the ID of this container using the ```docker container ls -a``` command.

Now, to create an image we need to “commit” this container. Commit creates an image locally on the system running the Docker engine. Run the following command, using the container ID you retrieved, in order to commit the container and create an image out of it.
```
docker container commit CONTAINER_ID
```
Once it has been commited, we can see the newly created image in the list of available images.

## Part 07 (Create an Image Using Dockerfile)

Create an image:
1. Create a `Dockerfile`. Sample of Dockerfile (OS: Ubuntu & Update and Install dependencies & Install python dependencies & Copy source code to /opt/ folder & Run the webserver using flask):
```
FROM Ubuntu

RUN apt update
RUN apt full-upgrade -y
RUN apt install python3,python3-pip -y

RUN pip install flask
RUN pip install flask-mysql

COPY . /opt/source-code

ENTRYPOINT FLASK_APP=/opt/source-code/app.py flask run
```
2. Create an image locally on system
```
docker build Dockerfile -t NAME
```
3. Make it available on the public DockerHub registry
```
docker push NAME
```
- **Dockerfile**
  - Format: `INSTRUCTION     argument`
  - Every instruction is in UPPERCASE format in an instruction -> `FROM, RUN, COPY, ...`
  - Everything in front of an instruction is an argument to that instruction.
  - `FROM`: First line of dockerfile defines what the base OS should be for this container or another image that was created before which is based on an OS. It's mandatory command for dockerfile.
  - `RUN`: Instructs docker to run a particular command on those based images.
  - `COPY`: Copy whatever you need inside the docker image
  - `ENTRYPOINT`: Allows us to specify a command that will be run when the image is run as a container.
- Layered Architecture
  - When docker builds images it builds these in a layered architecture. Each line of instruction creates a new layer in the docker image with just the changes from the previous layer. You can see size of packages with this command:
  ```
  docker history NAME
  ```
  - When you run the command below, you can see the various steps involved and the result of each task. All the layers build are cast so the layered architecture helps you restart docker build from that particular step in case it fails or if you were to add new steps in the build process, you wouldn't have to start all over again.
  ```
  docker build .
  ```
  - All the layers are build cached by docker. So if you wanted to add another step to dockerfile or there was a failure in a step, you can rebuild it. If you add a step to dockerfile, only layers above the updated layer need to rebuild.
  ```
  docker build Dockerfile -t NAME
  ```
- **Dockerignore**
  - Sometimes we need to exclude some files and folders before building our container to avoid unnecessarily sending large or sensitive files or directories to the daemon. So we make a file as `.dockerignore`
  - Think of it as `.gitignore` file for better understanding.
  - Syntax:
    1. Lines starting with `#` will be ignored.
    2. You can use wildcards with `*`
    3. You can use `?` like wildcards but for only a single character.
    4. Lines starting with `!` can be used to make exceptions to exclusions. But it must come after that specific ignore files.
- What can you containerize?
  - Databases
  - Development tools
  - Operating Systems
  - All of the applications
    - Browsers
    - Utility (Curl, ...)
    - Spotify
    - Skype
  - EVERYTHING :)

A wise man once said:

>Give a sysadmin an image and their app will be up-to-date for a day, give a sysadmin a Dockerfile and their app will always be up-to-date.

Hence, creating images for the complex applications is prefered to use Dockerfiles rather than commiting into local volume and manipulating the result. 

## Part 08 (CMD vs ENTRYPOINT)


## Part 09 (Networking)

- List of network adaptors which docker use to provide inter/intra connections between the containers and outside world (edge network adaptor) `-a`
```
docker network
docker network ls
```
## Part 10 (Storage)


## Part 11 (Compose)


## Part 12 (Registry)


## Part 13 (Engine)


## Part 14 (Docker Orchestration)

In many applications, running a single service in a single machine will do the job, But production applications are usually much more complex and the single server model will not work to due to vaious reasons like container creation delay, ensuring [high availability](https://en.wikipedia.org/wiki/High_availability) and the ability to scale. For production applications IT users and app teams need more sophisticated tools. Docker supplies two such tools: Docker Swarm and Kubernetes. 

### Chapter 1 (Docker Swarm)


### Chapter 2 (Kubernetes)


## Part 15 (Conclusion)

## Part 16 (Terminology)

- **Images**: The file system and configuration of our application which are used to create containers.
- **Containers**: Running instances of Docker images.
- **Registry**: A server side application that stores and lets you download Docker images. 
- **Docker Hub**: A registry of Docker images.
- **Layers**: A Docker image is built up from a series of layers. Each layer represents an instruction in the image’s Dockerfile. Each layer except the last one is read-only.
- **Dockerfile**:  A text file that contains all the commands, in order, needed to build a given image.

## Part 17 (References)

1. [Docker Documentation Samples](https://docs.docker.com/samples/)
2. [Play with Docker](https://training.play-with-docker.com/)
