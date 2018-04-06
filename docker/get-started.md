## Hands-on Docker
### Clone the LAMMPS docker container
* To clone the LAMMPS container: `docker pull cbgeo/lammps:latest`. The tag `latest` is not required.  This clones the `cbgeo/lammps` container from the [Docker Hub](https://hub.docker.com).

`docker pull cbgeo/lammps`

```shell
Using default tag: latest
Trying to pull repository docker.io/cbgeo/lammps ... 
sha256:bae487335973fb836b209f3834fab17093bbc7d3060b475322d7c0da2fb0e798: Pulling from docker.io/cbgeo/lammps
d5e46245fe40: Pull complete 
6fc0632c5d83: Pull complete 
997f11b9bffd: Pull complete 
Digest: sha256:bae487335973fb836b209f3834fab17093bbc7d3060b475322d7c0da2fb0e798
Status: Downloaded newer image for docker.io/cbgeo/lammps:latest
```

### Running the LAMMPS image
* To launch the `cbgeo/lammps` docker container, run `docker run -ti cbgeo/lammps:latest /bin/bash`

Once logged in you can run `/opt/lammps/src/lmp_serial -i /opt/lammps/examples/granregion/in.granregion.mixer`

### Running a container with a local volume mounted
* `docker run -ti -v /home/<user>/<mounted-folder>/:/<path-in-container> cbgeo/lammps:latest`

Users running Linux distributions with SELinux enabled (Redhat, CentOS, Fedora, and others) will need to add the `:z` option to all subsequent host volume mounts `-v`, e.g.: `docker run -it -v $(pwd):/home/cbgeo/research/lem-shared/:z quay.io/cbgeo/lem:latest /bin/bash.`

### Connecting to a running container
* `docker exec -ti <containerid> /bin/bash`

### Start / stop a container
* `docker start <containerid>`
* `docker stop <containerid>`

### Delete a container
* `docker rm <containerid>`, stop the container before deleting it. 
* To delete all docker containers `docker rm $(docker ps -a -q)`

### To login as root
* Launching docker as root user: `docker exec -u 0 -ti <containerid> /bin/bash`

### Exposing ports
* Let's run the tensor flow container `docker run -ti tensorflow/tensorflow`.

* However, when we ran it the host and the container ports are not bound.

* To connect to a particular port (for e.g., 8888) in docker container to port `8888` in localhost:
	`docker run -ti -p 8888:8888 tensorflow/tensorflow`
