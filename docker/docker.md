# Docker
[Docker](https://www.docker.com/) is one of the most popular container technology, which automates the deployment of applications by packaging an application with all of its dependencies into software containers.  Download the Docker engine [here](https://docs.docker.com/engine/installation/). The difference between docker containers and typical Virtual Machines (VMs): 

![Docker v VM](docker-vm.png)

> The following section is from [A beginners' guide to Docker, Container and VMs](https://medium.freecodecamp.org/a-beginner-friendly-introduction-to-containers-vms-and-docker-79a9e3e119b) 

## What are containers and VMs?
Containers and VMs are similar in their goals: to isolate an application and its dependencies into a self-contained unit that can run anywhere.

Moreover, containers and VMs remove the need for physical hardware, allowing for more efficient use of computing resources, both in terms of energy consumption and cost-effectiveness.

The main difference between containers and VMs is in their architectural approach. Let’s take a closer look.

### Virtual Machines
A VM is essentially an emulation of a real computer that executes programs like a real computer. VMs run on top of a physical machine using a 'hypervisor'. A hypervisor, in turn, runs on either a host machine or on 'bare-metal'.

Let’s unpack the jargon:

A hypervisor is a piece of software, firmware, or hardware that VMs run on top of. The hypervisors themselves run on physical computers, referred to as the “host machine”. The host machine provides the VMs with resources, including RAM and CPU. These resources are divided between VMs and can be distributed as you see fit. So if one VM is running a more resource heavy application, you might allocate more resources to that one than the other VMs running on the same host machine.

The VM that is running on the host machine (again, using a hypervisor) is also often called a “guest machine.” This guest machine contains both the application and whatever it needs to run that application (e.g. system binaries and libraries). It also carries an entire virtualized hardware stack of its own, including virtualized network adapters, storage, and CPU — which means it also has its own full-fledged guest operating system. From the inside, the guest machine behaves as its own unit with its own dedicated resources. From the outside, we know that it’s a VM — sharing resources provided by the host machine.

As mentioned above, a guest machine can run on either a hosted hypervisor or a bare-metal hypervisor. There are some important differences between them.

First off, a hosted virtualization hypervisor runs on the operating system of the host machine. For example, a computer running OSX can have a VM (e.g. VirtualBox or VMware Workstation 8) installed on top of that OS. The VM doesn’t have direct access to hardware, so it has to go through the host operating system (in our case, the Mac’s OSX).

The benefit of a hosted hypervisor is that the underlying hardware is less important. The host’s operating system is responsible for the hardware drivers instead of the hypervisor itself, and is therefore considered to have more "hardware compatibility". On the other hand, this additional layer in between the hardware and the hypervisor creates more resource overhead, which lowers the performance of the VM.

A bare-metal hypervisor environment tackles the performance issue by installing on and running from the host machine’s hardware. Because it interfaces directly with the underlying hardware, it doesn’t need a host operating system to run on. In this case, the first thing installed on a host machine’s server as the operating system will be the hypervisor. Unlike the hosted hypervisor, a bare-metal hypervisor has its own device drivers and interacts with each component directly for any I/O, processing, or OS-specific tasks. This results in better performance, scalability, and stability. The tradeoff here is that hardware compatibility is limited because the hypervisor can only have so many device drivers built into it.

After all this talk about hypervisors, you might be wondering why we need this additional "hypervisor" layer in between the VM and the host machine at all.

Well, since the VM has a virtual operating system of its own, the hypervisor plays an essential role in providing the VMs with a platform to manage and execute this guest operating system. It allows for host computers to share their resources amongst the virtual machines that are running as guests on top of them.

![VM](vm.png)

## Images and containers
A container is launched by running an image. An *image* is an executable package that includes everything needed to run an application--the code, a runtime, libraries, environment variables, and configuration files.

A *container* is a runtime instance of an image--what the image becomes in memory when executed (that is, an image with state, or a user process). You can see a list of your running containers with the command, docker ps, just as you would in Linux.


## Docker useful commands
### Clone a docker container
* `docker pull <image>:latest`. The tag `latest` is not required. Clone the container appropriate for your use case, for eg., use `centos` for the latest version of CentOS. CentOS and most other linux operating systems are available as official repositories from the [Docker Hub](https://hub.docker.com).

* To use non-official docker images, you need to add the organisation before the docker image, for example to pull the latest version of `tensorflow`: `docker pull tensorflow/tensorflow:latest`

### Using a docker image
* The docker image can be used directly from the Docker Hub
* To launch the `centos`  docker container, run `docker run -ti tensorflow/tensorflow:latest /bin/bash`

### Running a container with a local volume mounted
* `docker run -ti -v /home/<user>/<mounted-folder>/:/<path-in-container> cbgeo/cb-geo:latest`

### Connecting to a running container
* `docker exec -ti <containerid> /bin/bash`

### Start / stop a container
* `docker start <containerid>`
* `docker stop <containerid>`

### Delete a container
* `docker rm <containerid>`, stop the container before deleting it. 
* To delete all docker containers `docker rm $(docker ps -a -q)`

### Exposing ports
* To connect to a particular port (for e.g., 3000) in docker container to port `3000` in localhost:
	`docker run -ti -p 3000:3000 tensorflow/tensorflow`

### To login as root
* Launching docker as root user: `docker exec -u 0 -ti <containerid> /bin/bash`

### Creating an image from a docker file

* To build an image from docker file run as root `docker build -t "tensorflow/tensorflow" /path/to/Dockerfile`
* `docker history` will show you the effect of each command has on the overall size of the file.


# Docker learning materials

[A beginners' guide to Docker, Container and VMs](https://medium.freecodecamp.org/a-beginner-friendly-introduction-to-containers-vms-and-docker-79a9e3e119b)

[Docker getting started](https://docs.docker.com/get-started/)

[An introduction to docker](http://prakhar.me/docker-curriculum/).

[Docker self-paced training](https://training.docker.com/)

[Docker quick reference sheet](docker-quick-ref.pdf)

![Docker cheat sheet](docker-cheat-sheet.png)
