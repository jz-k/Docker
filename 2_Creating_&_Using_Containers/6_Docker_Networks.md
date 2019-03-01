# Docker Networks

## Table of Contents
- [Docker Networks](#docker-networks)
	- [Table of Contents](#table-of-contents)
		- [Docker Network Defaults](#docker-network-defaults)
		- [_"Batteries included, but removable"_](#%22batteries-included-but-removable%22)
		- [Code Time](#code-time)
		- [Networking Limitations](#networking-limitations)
	
### Docker Network Defaults
- Each container is connected to a private virtual network "bridge"
- Each virtual network routes through NAT firewall on host IP
	* Docker daemon configures host IP on its default interface, so containers can "get out" to rest of internet
- All containers on a virtual network can talk to each other without -p
- Best practice is to create a new virtual network for each app:
	* network "my_web_app" for mysql and php/apache containers
	* network "my_api" for mongo and nodejs containers
	* Above networks can talk within each other without -p, but require -p to talk between each other
### _"Batteries included, but removable"_
	* Defaults work well in many cases, but Docker makes it easy to swap out parts and customize
		- Can create multiple virtual networks per app
		- Can attach containers to multiple virtual networks
		- Can attach containers to _no_ network
		- Can skip virtual networks and use host IP instead (--net=host)
			* Will lose some containerization benefits (but may still be required)
		- Can use different Docker network drivers to gain new abilities
### Code Time
```
docker container run -p 80:80 --name webhost -d nginx
// --public/-p : remember, publishing ports is always in HOST:CONTAINER format
docker container port webhost	// shows which ports are forwarding traffic from host into container itself
docker container inspect --format '{{ .NetworkSettings.IPAddress }}' webhost	// gets IP of container
// --format : a common option for formatting the output of commands using "Go templates"
// .NetworkSettings.IPAddress is the actual node of the JSON output we want to see
```
### Networking Limitations
- Can't set a HOST:CONTAINER relationship on same HOST port between two different containers
	* If, say, port 80 was already used for the HOST to listen for traffic from CONTAINER port 8081 (e.g. 80:8081), it can't also listen for traffic from a CONTAINER using port 8082 (e.g. 80:8082)
		- trying to publish the second container would spur an error
			* this limitation is not Docker-specific--it is inherent in the way IP Networking works