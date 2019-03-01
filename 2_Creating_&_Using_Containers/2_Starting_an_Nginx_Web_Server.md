# Nginx

## Table of Contents
- [Nginx](#nginx)
	- [Table of Contents](#table-of-contents)
		- [Image vs Container](#image-vs-container)
		- [Nginx Web Server](#nginx-web-server)
		- [Starting a Container](#starting-a-container)
		- [Stopping a Container](#stopping-a-container)
		- [Running Container in Background](#running-container-in-background)
		- [docker container ls](#docker-container-ls)
		- [docker container stop <someID>](#docker-container-stop-someid)
		- [run vs. start](#run-vs-start)
		- [Specifying a Container Name](#specifying-a-container-name)
		- [Accessing Logs of Background Containers](#accessing-logs-of-background-containers)
		- [docker container top CONTAINER <containerName>](#docker-container-top-container-containername)
		- [Surfacing Options](#surfacing-options)
		- [Removing Containers](#removing-containers)
		- [What Happens When A Container Runs](#what-happens-when-a-container-runs)
		- [Changing Defaults When Starting an Image](#changing-defaults-when-starting-an-image)
	
### Image vs Container

| Image                       | Container                                  |
| --------------------------- | ------------------------------------------ |
| Application to be run       | Instance of an image, running as a process |
| The 'model' for a container | Many can be based off the same image       |
| Analogous to a Class        | Analogous to an instance of a class        |

### Nginx Web Server
- An Image
- Docker's default image "registry" is Docker Hub
	* Kind of like what github is to source code, Docker Hub is to container images
### Starting a Container
```
docker container run --publish 80:80 nginx			// runs in foreground
```
- Can then go to localhost -- nginx server should be listening!
- So what happens in that command?
	1. Docker looks for an image called 'nginx'
	2. Docker pulls the latest image for 'ngnix' from Docker Hub
	3. Starts ngnix as a new process (container)
	4. Opens port 80 of host IP
	5. Routes all traffic to port 80 to the container IP, port 80
		* If the host port (the left number) is in use by anything else, a "bind" error will occur
		* Any port can be specified for use on left side; if "80" were in use, then 8080 could be used
			- In this case, visit "localhost:8080" to access the nginx server
### Stopping a Container
- ctrl+c
	* Sends a stop signal to whatever process is running in foreground
		- On Windows this can cause issues; the foreground may be exited but container may remain running in background
### Running Container in Background
```
docker container run --publish 80:80 --detach nginx
```
- Starts container in background
- Returns unique container ID
### docker container ls
- By default, lists containers currently in use (i.e. running)
- adding option "-a" will show all containers whether or not they are in use
	```
	docker container ls -a
	```
### docker container stop <someID>
- The entire ID doesn't need to be entered; first few characters should be enough--however many to uniquely identify the container!
- Example:
	```
	docker container stop 690
	```
- Running docker container ls after stopping all containers should surface an empty list
	* docker container ls -a will still surface all containers, whether or not they are currently running
### run vs. start
```
docker container run				// always starts a *new* container
docker container start				// starts an existing container which is currently stopped
```
### Specifying a Container Name
```
docker container run --publish 80:80 --detach --name webhost nginx
```
- Running docker container ls following the above command will show the container listed with the name "webhost"
- Name is set to whatever word follows the "--name" flag
- Name is always required on a container (and must be unique), but if one is not specified, Docker generates a unique name automatically
	* Generated names in format adjective-hackerOrComputerScientistName
### Accessing Logs of Background Containers
```
docker container logs <containerName>
// e.g.:
docker container logs webhost
```
- Above command surfaces logs for the named container
```
docker logs --help			// surfaces available log commands
```
### docker container top CONTAINER <containerName>
- Displays the running processes of a container
- Example:
  ```
  docker container top CONTAINER webhost
  ```
### Surfacing Options
```
docker container --help
```
### Removing Containers
```
docker container rm <containerID> [<containerIDs...>]
```
- Can remove one or more containers
- Example:
```
docker container rm 63f 690 0de			// only need to put enough of the ID to uniquely identify each container
```
- Returns the provided IDs of containers that were removed
- Running containers cannot be removed
	* Can force active containers to be removed with flag "-f":
		```
		docker container rm -f 0de
		```
### What Happens When A Container Runs
- On docker container run...
	1. Docker looks for the image locally in image cache
		1. If image is not found, Docker looks in remote image repository (default repo: Docker Hub)
		2. Downloads the latest image (by default) from remote repository, then stores in local image cache
			* Can specify a version after the image name by connecting the image and version with a ":"
				```
				// e.g.
				nginx:latest
				```
	2. Creates new container based on that image and prepares to start
	3. Gives container a virtual IP on a private network, inside Docker
	4. Opens up a given port on host and forwards to given port in container:
		```
		// e.g.:
		80:80		// first number is host post, second is container port to forward to
		```
	5. Starts container by using the CMD in the image Dockerfile
### Changing Defaults When Starting an Image
```
							// set host port 8080			// change image version to use ("use v1.11")
docker container run --publish 8080:80 --name webhost -d nginx:1.11 nginx -T
																	// change CMD run on start
```
