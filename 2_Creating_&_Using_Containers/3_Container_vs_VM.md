# Container vs VM

## Table of Contents
- [Container vs VM](#container-vs-vm)
	- [Table of Contents](#table-of-contents)
		- [So What is a Container?](#so-what-is-a-container)
		- [Getting a Shell in the Docker for Windows/Mac MobyVM](#getting-a-shell-in-the-docker-for-windowsmac-mobyvm)
		- [Example Container Process](#example-container-process)
		- [Windows Containers](#windows-containers)
	
### So What is a Container?
- Containers **are not** mini-VMs
	* They are just processes
- Limited to whatever resources they can access
- Exit when process stops
- Less like a virtual machine and more like a restricted process on the host machine
### Getting a Shell in the Docker for Windows/Mac MobyVM
- See [here](https://www.bretfisher.com/getting-a-shell-in-the-docker-for-windows-vm/) and [here](https://github.com/justincormack/nsenter1)
- Since Win/MacOS versions of Docker actually _do_ run a small VM (though it is not a full OS), there are extra steps that must be taken to access the processes within the MobyVM Docker uses to run its containers
### Example Container Process
```
docker run --name mongo -d mongo		// runs mongo! could be handy
docker ps 								// lists processes (could also use docker container ls)
ps aux 				// list all processes on system (mongod should be listed--a process on the host!) (must be "in" the VM if on Win/MacOS)
docker stop mongo
ps aux				// mongod no longer listed! (process has been stopped)
```
### Windows Containers
- Not much info in the course, but some resources:
	* [Windows Containers and Docker 101](https://www.youtube.com/watch?v=066-9yw8-7c)
	* [The Path to Windows and Linux Parity in Docker](https://www.youtube.com/watch?v=4ZY_4OeyJsw)
	* [Docker + Microsoft -- Investing in the Future of Your Application](https://www.youtube.com/watch?v=QASAqcuuzgI)