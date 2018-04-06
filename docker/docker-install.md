# Install Docker

To [install docker](https://docs.docker.com/engine/installation/) follow the instructions on this [page](https://docs.docker.com/engine/installation/). 

# Test Docker version
Run `docker --version` and ensure that you have a supported version of Docker:

```
docker --version
```

Docker version 17.12.0-ce, build c97c6d6
Run `docker info` or (`docker version` without `--`) to view even more details about your docker installation:

```
docker info

Containers: 0
 Running: 0
 Paused: 0
 Stopped: 0
Images: 0
Server Version: 17.12.0-ce
Storage Driver: overlay2
...
```

To avoid permission errors (and the use of sudo), add your user to the docker group. 

# Test Docker installation

1. Test that your installation works by running the simple Docker image, hello-world:

```
docker run hello-world
```
```
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
ca4f61b1923c: Pull complete
Digest: sha256:ca0eeb6fb05351dfc8759c20733c91def84cb8007aa89a5bf606bc8b315b9fc7
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.
...
```

2. List the hello-world image that was downloaded to your machine:
```
docker image ls
```
3. List the hello-world container (spawned by the image) which exits after displaying its message. If it were still running, you would not need the --all option:

```
docker container ls --all
```
```
CONTAINER ID     IMAGE           COMMAND      CREATED            STATUS
54f4984ed6a8     hello-world     "/hello"     20 seconds ago     Exited (0) 19 seconds ago
```

## Recap and cheat sheet
```md
## List Docker CLI commands
docker
docker container --help

## Display Docker version and info
docker --version
docker version
docker info

## Execute Docker image
docker run hello-world

## List Docker images
docker image ls

## List Docker containers (running, all, all in quiet mode)
docker container ls
docker container ls --all
docker container ls -aq
```

