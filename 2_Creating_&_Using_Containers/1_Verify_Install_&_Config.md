# Verify Docker Install and Config

## Table of Contents
- [Verify Docker Install and Config](#verify-docker-install-and-config)
	- [Table of Contents](#table-of-contents)
		- [docker version](#docker-version)
		- [docker info](#docker-info)
		- [docker](#docker)
	
### docker version
- Returns version of client and server
- Want to make sure client/server are as recent as possible
- If data is returned from server, then the server is running and can be reached--good!
### docker info
- Displays most configuration values for Docker
### docker
- Displays most commands available to Docker
- Management commands are used to organize direct Docker commands:
	```
	docker <command> <sub-command>	(options)
	```
- But the old way still works:
	```
	docker <command> (options)
	```
- Command example:
	```
	docker container run		//  new way
	docker run					// old way
	```
	* Both commands above accomplish the same thing
		- Old commands will continue to work the "old" way
		- New + future commands require the high-level command before issuing the sub-command--to help keep things organized