# Dockerfile
A Dockerfile is where you write the instructions to build a Docker image. These instructions can be:

* `RUN apt-get y install some-package`: to install a software package
* `EXPOSE 8000`: to expose a port
* `ENV ANT_HOME /usr/local/apache-ant` to pass an environment variable
and so forth. Once you’ve got your Dockerfile set up, you can use the docker build command to build an image from it.

## Define a container with a `Dockerfile`

Dockerfile defines what goes on in the environment inside your container. Access to resources like networking interfaces and disk drives is virtualized inside this environment, which is isolated from the rest of your system, so you need to map ports to the outside world, and be specific about what files you want to “copy in” to that environment. However, after doing that, you can expect that the build of your app defined in this Dockerfile behaves exactly the same wherever it runs.

```markdown
# Use an official Python runtime as a parent image
FROM python:2.7-slim

# Set the working directory to /app
WORKDIR /app

# Copy the current directory contents into the container at /app
ADD . /app

# Install any needed packages specified in requirements.txt
RUN pip install --trusted-host pypi.python.org -r requirements.txt

# Make port 80 available to the world outside this container
EXPOSE 80

# Define environment variable
ENV NAME World

# Run app.py when the container launches
CMD ["python", "app.py"]
```


## Creating a LAMMPS Dockerfile

Create an empty directory. Change directories (cd) into the new directory, create a file called `Dockerfile`, copy-and-paste the following content into that file, and save it. Take note of the comments that explain each statement in your new Dockerfile.

```
FROM centos:latest
MAINTAINER Krishna Kumar <kks32@cam.ac.uk>

# Update to latest packages and development tools
RUN yum update -y && \
    yum -y groupinstall "Development Tools" && \
    yum install -y wget && \
    yum clean all

RUN cd /opt && wget http://lammps.sandia.gov/tars/lammps-stable.tar.gz
RUN mkdir -p /opt/lammps && cd /opt/lammps && \
    tar xf ../lammps-stable.tar.gz --strip-components 1 && \
    cd src && \
    make yes-granular |& tee log.make_yes_granular && \
    make -j serial |& tee log.make_serial

# Change to lammps directory
# ENTRYPOINT ["/opt/lammps/src/lmp_serial", "-i"]
# CMD ["/opt/lammps/examples/granregion/in.granregion.mixer"]
```

Now run the build command. This creates a Docker image, which we’re going to tag using `-t` so it has a friendly name.

`docker build -t lammps .`

Where is your built image? It’s in your machine’s local Docker image registry:

`docker image ls`

## Share your image
To demonstrate the portability of what we just created, let’s upload our built image and run it somewhere else. After all, you need to know how to push to registries when you want to deploy containers to production.

A registry is a collection of repositories, and a repository is a collection of images—sort of like a GitHub repository, except the code is already built. An account on a registry can create many repositories. The docker CLI uses Docker’s public registry by default.

Note: We use Docker’s public registry here just because it’s free and pre-configured, but there are many public ones to choose from, and you can even set up your own private registry using Docker Trusted Registry.

### Log in with your Docker ID
If you don’t have a Docker account, sign up for one at cloud.docker.com. Make note of your username.

Log in to the Docker public registry on your local machine.

```
$ docker login
```

### Tag the image
The notation for associating a local image with a repository on a registry is username/repository:tag. The tag is optional, but recommended, since it is the mechanism that registries use to give Docker images a version. Give the repository and tag meaningful names for the context, such as get-started:part2. This puts the image in the get-started repository and tag it as part2.

Now, put it all together to tag the image. Run docker tag image with your username, repository, and tag names so that the image uploads to your desired destination. The syntax of the command is:

`docker tag image username/repository:tag`

For example:

`docker tag lammps kks32/lammps:version1`

Run `docker image ls` to see your newly tagged image.

```
REPOSITORY               TAG                 IMAGE ID            CREATED             SIZE
lammps                   latest              d9e555c53008        3 minutes ago       195MB
kks32/lammps             version1            d9e555c53008        3 minutes ago       195MB
python                   2.7-slim            1c7128a655f6        5 days ago          183MB
...
```

### Publish the image

Upload your tagged image to the repository:

`docker push username/repository:tag`

Once complete, the results of this upload are publicly available. If you log in to Docker Hub, you see the new image there, with its pull command.


